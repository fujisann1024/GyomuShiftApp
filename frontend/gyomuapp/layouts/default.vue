<template>
    <div class="flex flex-col min-h-screen">
        <header class="bg-blue-500 text-white text-center p-4">
            サンプルアプリ
        </header>
        <main class="flex-grow p-4">
            <slot />
        </main>
        <footer class="bg-blue-500 text-white text-center p-4">
            <p>© 2021 サンプルアプリ</p>
        </footer>
    </div>
</template>