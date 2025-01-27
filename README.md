自治会DXアプリにおいて、管理者アカウントを「メールアドレスによる招待制」にする方法を、Laravelをベースに解説します。

1. 招待機能の概要

管理者（users テーブル）のアカウント作成を、管理者からの招待メールを受け取ったユーザーのみが行えるようにします。具体的な流れは以下の通りです：
	1.	既存の管理者が新しい管理者を招待
	•	メールアドレスを入力し、招待メールを送信
	•	招待のトークンを生成しデータベースに保存
	2.	招待を受けたユーザーがリンクから登録
	•	招待メールのリンクを開く
	•	トークンを確認し、有効であればパスワードを設定しアカウント作成

2. データベース構成

以下のような admin_invitations テーブルを作成し、招待情報を管理します。

Schema::create('admin_invitations', function (Blueprint $table) {
    $table->id();
    $table->string('email')->unique();
    $table->string('token')->unique();
    $table->boolean('used')->default(false);
    $table->timestamps();
});

3. 招待の実装手順

3.1 招待メール送信の実装

コントローラーに招待処理を実装します。

use App\Models\AdminInvitation;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Mail;
use Illuminate\Support\Str;

class AdminInvitationController extends Controller
{
    public function invite(Request $request)
    {
        $request->validate([
            'email' => 'required|email|unique:users,email|unique:admin_invitations,email',
        ]);

        $token = Str::random(32);

        // 招待情報を保存
        AdminInvitation::create([
            'email' => $request->email,
            'token' => $token,
        ]);

        // 招待メール送信
        Mail::to($request->email)->send(new \App\Mail\AdminInvitationMail($token));

        return response()->json(['message' => '招待メールを送信しました。']);
    }
}

3.2 招待メールの内容

app/Mail/AdminInvitationMail.php を作成。

namespace App\Mail;

use Illuminate\Mail\Mailable;

class AdminInvitationMail extends Mailable
{
    public $token;

    public function __construct($token)
    {
        $this->token = $token;
    }

    public function build()
    {
        return $this->subject('管理者アカウント招待')
                    ->view('emails.admin_invitation')
                    ->with([
                        'url' => url('/register?token=' . $this->token),
                    ]);
    }
}

メールビュー（resources/views/emails/admin_invitation.blade.php）:

<p>管理者アカウントの招待を受けました。</p>
<p>以下のリンクから登録を完了してください。</p>
<a href="{{ $url }}">登録リンク</a>

4. 招待トークンを使用した登録処理

AuthController に、招待されたユーザーがアカウントを作成する処理を追加します。

use App\Models\AdminInvitation;
use App\Models\User;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Hash;

class AuthController extends Controller
{
    public function showRegistrationForm(Request $request)
    {
        $token = $request->query('token');
        $invitation = AdminInvitation::where('token', $token)->where('used', false)->first();

        if (!$invitation) {
            return redirect('/')->with('error', '招待リンクが無効です。');
        }

        return view('auth.register', ['email' => $invitation->email, 'token' => $token]);
    }

    public function register(Request $request)
    {
        $request->validate([
            'password' => 'required|min:8|confirmed',
            'token' => 'required|exists:admin_invitations,token',
        ]);

        $invitation = AdminInvitation::where('token', $request->token)->where('used', false)->first();

        if (!$invitation) {
            return redirect('/register')->with('error', '無効な招待リンクです。');
        }

        // ユーザー作成
        User::create([
            'email' => $invitation->email,
            'password' => Hash::make($request->password),
        ]);

        // 招待を使用済みにする
        $invitation->update(['used' => true]);

        return redirect('/login')->with('success', '登録が完了しました。');
    }
}

登録ページビュー（resources/views/auth/register.blade.php）:

<form method="POST" action="{{ route('register') }}">
    @csrf
    <input type="hidden" name="token" value="{{ $token }}">
    <div>
        <label>Email:</label>
        <input type="email" name="email" value="{{ $email }}" readonly>
    </div>
    <div>
        <label>パスワード:</label>
        <input type="password" name="password">
    </div>
    <div>
        <label>パスワード確認:</label>
        <input type="password" name="password_confirmation">
    </div>
    <button type="submit">登録</button>
</form>

5. ルーティング設定

routes/web.php に以下のルートを追加します。

use App\Http\Controllers\AdminInvitationController;
use App\Http\Controllers\AuthController;

Route::middleware('auth')->group(function () {
    Route::post('/invite', [AdminInvitationController::class, 'invite'])->name('invite');
});

Route::get('/register', [AuthController::class, 'showRegistrationForm'])->name('register.form');
Route::post('/register', [AuthController::class, 'register'])->name('register');

6. アクセス制御

管理者画面へのアクセス制限を auth ミドルウェアで制御し、認証済みユーザーのみアクセスできるようにします。

Route::middleware(['auth'])->group(function () {
    Route::get('/dashboard', function () {
        return view('admin.dashboard');
    })->name('dashboard');
});

7. フロントエンド（管理者招待ボタンの追加）

管理者用ダッシュボードに招待フォームを追加:

<form action="{{ route('invite') }}" method="POST">
    @csrf
    <input type="email" name="email" placeholder="招待するメールアドレス">
    <button type="submit">招待</button>
</form>

8. まとめ

この実装により、管理者アカウント作成は以下の流れで進みます。
	1.	既存管理者が新管理者を招待（メール送信）
	2.	招待されたユーザーがリンクから登録ページにアクセス
	3.	招待トークンを検証し、アカウントを作成
	4.	招待状を使用済みにする

これにより、セキュアかつ管理しやすい管理者招待フローが実現できます。
