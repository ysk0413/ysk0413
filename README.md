<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>管理者ダッシュボード</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/alpinejs" defer></script>
</head>
<body class="bg-gray-100 font-sans leading-normal tracking-normal">

    <!-- サイドバーメニュー -->
    <div x-data="{ open: false }" class="flex">
        <!-- サイドバー -->
        <aside :class="open ? 'w-64' : 'w-16'" class="h-screen bg-gray-800 text-white flex flex-col transition-all duration-300">
            <div class="flex items-center justify-between p-4">
                <span class="text-xl font-bold" x-show="open">管理メニュー</span>
                <button @click="open = !open">
                    <svg x-show="!open" class="w-6 h-6" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path d="M4 6h16M4 12h16M4 18h16"></path>
                    </svg>
                    <svg x-show="open" class="w-6 h-6" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path d="M6 18L18 6M6 6l12 12"></path>
                    </svg>
                </button>
            </div>
            <nav class="flex-1">
                <a href="#" class="block py-3 px-4 hover:bg-gray-700">ダッシュボード</a>
                <a href="#" class="block py-3 px-4 hover:bg-gray-700">会員管理</a>
                <a href="#" class="block py-3 px-4 hover:bg-gray-700">回覧板管理</a>
                <a href="#" class="block py-3 px-4 hover:bg-gray-700">クーポン管理</a>
                <a href="#" class="block py-3 px-4 hover:bg-gray-700">決済管理</a>
            </nav>
        </aside>

        <!-- メインコンテンツ -->
        <main class="flex-1 p-6">
            <h1 class="text-3xl font-bold mb-4">管理者ダッシュボード</h1>

            <!-- KPIカード -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <div class="bg-white p-6 rounded-lg shadow-md">
                    <h2 class="text-lg font-semibold text-gray-700">登録会員数</h2>
                    <p class="text-4xl font-bold text-blue-500">1,234</p>
                </div>
                <div class="bg-white p-6 rounded-lg shadow-md">
                    <h2 class="text-lg font-semibold text-gray-700">未納会費数</h2>
                    <p class="text-4xl font-bold text-red-500">12</p>
                </div>
                <div class="bg-white p-6 rounded-lg shadow-md">
                    <h2 class="text-lg font-semibold text-gray-700">最新回覧板投稿</h2>
                    <p class="text-4xl font-bold text-green-500">5</p>
                </div>
            </div>

            <!-- 最近のアクティビティ -->
            <div class="mt-8 bg-white p-6 rounded-lg shadow-md">
                <h2 class="text-xl font-bold mb-4">最近の活動</h2>
                <ul>
                    <li class="border-b py-2">新しい会員が登録されました（1時間前）</li>
                    <li class="border-b py-2">回覧板「自治会総会のお知らせ」が投稿されました（3時間前）</li>
                    <li class="border-b py-2">会員「田中さん」がクーポンを使用しました（昨日）</li>
                </ul>
            </div>

            <!-- タブ切り替え -->
            <div class="mt-8 bg-white p-6 rounded-lg shadow-md" x-data="{ tab: 'summary' }">
                <div class="flex border-b">
                    <button @click="tab = 'summary'" :class="tab === 'summary' ? 'text-blue-500 border-b-2 border-blue-500' : 'text-gray-700'" class="py-2 px-4">
                        概要
                    </button>
                    <button @click="tab = 'details'" :class="tab === 'details' ? 'text-blue-500 border-b-2 border-blue-500' : 'text-gray-700'" class="py-2 px-4">
                        詳細
                    </button>
                </div>
                <div x-show="tab === 'summary'" class="mt-4">
                    <p>ここに概要の内容を表示します。</p>
                </div>
                <div x-show="tab === 'details'" class="mt-4">
                    <p>ここに詳細の内容を表示します。</p>
                </div>
            </div>

        </main>
    </div>

</body>
</html>
