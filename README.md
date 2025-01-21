<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>管理者ダッシュボード</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/alpinejs" defer></script>
</head>
<body class="bg-gray-100 font-sans leading-relaxed text-black">

    <div x-data="{ open: false }" class="flex">
        <!-- サイドバー -->
        <aside :class="open ? 'w-72' : 'w-24'" class="h-screen bg-blue-600 text-white flex flex-col transition-all duration-300 shadow-md">
            <div class="flex items-center justify-between p-6">
                <span class="text-2xl font-bold" x-show="open">管理メニュー</span>
                <button @click="open = !open">
                    <svg x-show="!open" class="w-10 h-10" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                        <path d="M4 6h16M4 12h16M4 18h16"></path>
                    </svg>
                    <svg x-show="open" class="w-10 h-10" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                        <path d="M6 18L18 6M6 6l12 12"></path>
                    </svg>
                </button>
            </div>
            <nav class="flex-1 text-lg">
                <a href="#" class="block py-4 px-6 hover:bg-blue-500 flex items-center">
                    <span class="material-icons mr-4">home</span> ダッシュボード
                </a>
                <a href="#" class="block py-4 px-6 hover:bg-blue-500 flex items-center">
                    <span class="material-icons mr-4">group</span> 会員管理
                </a>
                <a href="#" class="block py-4 px-6 hover:bg-blue-500 flex items-center">
                    <span class="material-icons mr-4">announcement</span> 回覧板管理
                </a>
                <a href="#" class="block py-4 px-6 hover:bg-blue-500 flex items-center">
                    <span class="material-icons mr-4">payment</span> 会費管理
                </a>
            </nav>
        </aside>

        <!-- メインコンテンツ -->
        <main class="flex-1 p-8">
            <h1 class="text-4xl font-bold mb-6">管理者ダッシュボード</h1>

            <!-- KPIカード -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                <div class="bg-white p-8 rounded-lg shadow-lg">
                    <h2 class="text-2xl font-bold text-gray-700">登録会員数</h2>
                    <p class="text-5xl font-bold text-blue-600 mt-4">1,234</p>
                </div>
                <div class="bg-white p-8 rounded-lg shadow-lg">
                    <h2 class="text-2xl font-bold text-gray-700">未納会費数</h2>
                    <p class="text-5xl font-bold text-red-600 mt-4">12</p>
                </div>
                <div class="bg-white p-8 rounded-lg shadow-lg">
                    <h2 class="text-2xl font-bold text-gray-700">最新回覧板投稿</h2>
                    <p class="text-5xl font-bold text-green-600 mt-4">5</p>
                </div>
            </div>

            <!-- 最近の活動 -->
            <div class="mt-12 bg-white p-8 rounded-lg shadow-lg">
                <h2 class="text-3xl font-bold mb-4">最近の活動</h2>
                <ul>
                    <li class="border-b py-4 text-xl">新しい会員が登録されました（1時間前）</li>
                    <li class="border-b py-4 text-xl">回覧板が投稿されました（3時間前）</li>
                </ul>
            </div>

            <!-- タブ切り替え -->
            <div class="mt-12 bg-white p-8 rounded-lg shadow-lg" x-data="{ tab: 'summary' }">
                <div class="flex border-b">
                    <button @click="tab = 'summary'" :class="tab === 'summary' ? 'text-blue-600 border-b-4 border-blue-600' : 'text-gray-700'" class="py-3 px-6 text-2xl">
                        概要
                    </button>
                    <button @click="tab = 'details'" :class="tab === 'details' ? 'text-blue-600 border-b-4 border-blue-600' : 'text-gray-700'" class="py-3 px-6 text-2xl">
                        詳細
                    </button>
                </div>
                <div x-show="tab === 'summary'" class="mt-6 text-xl">
                    <p>ここに概要の内容を表示します。</p>
                </div>
                <div x-show="tab === 'details'" class="mt-6 text-xl">
                    <p>ここに詳細の内容を表示します。</p>
                </div>
            </div>

        </main>
    </div>

</body>
</html>
