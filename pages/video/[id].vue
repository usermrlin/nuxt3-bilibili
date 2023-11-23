<template>
    <div>
        <h1>视频页</h1>
        <div>{{ $route.params.id }}</div>
    </div>
</template>

<script setup lang="ts">
// 引入Api
import { useRoute } from 'nuxt/app';
let { params } = useRoute()
console.log(params.id)
</script>

<style scoped></style>