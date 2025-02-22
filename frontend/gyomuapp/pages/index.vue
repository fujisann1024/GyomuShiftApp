<script setup lang="ts">
useHead({ title: "TOPページ"});
</script>
<template>
    <h1 class="font-black text-center text-3xl">こんにちは、Nuxt.js！</h1>
    <section>
        <NuxtLink to="/memberlist">会員</NuxtLink>
        <NuxtLink to="/shift">シフト</NuxtLink>
    </section>
</template>