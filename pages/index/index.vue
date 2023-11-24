<template>
    <!-- 公共头部 -->
    <AppHeader />
    <!-- 频道模块 -->
    <van-tabs>
        <van-tab v-for="item in channelList" :key="item.id" :title="item.name" />
    </van-tabs>
    <!-- 视频列表 -->
    <van-list v-model:loading="loading" :finished="finished" finished-text="去 bilibili App 看更多" @load="onLoad">
        <div class="video-list">
            <AppVideo v-for="item in list" :key="item.bvid" :item="item"></AppVideo>
        </div>
    </van-list>
</template>
<script setup lang="ts">
// 引入Api
import { useFetch } from 'nuxt/app';
import type { VideoItem } from '@/types/video'

// 获取tab栏列表
const { data: channelList } = await useFetch('/api/channel')

// 获取视频列表数据
const { data: videoList } = await useFetch('/api/video')
// 显示的列表
const list = ref<VideoItem[]>([])
// 加载状态
const loading = ref(false)
// 是否加载完成
const finished = ref(false)
// 滚动触底出发

// 页码
let page = 1
// 页数
let pageSize = 20
const onLoad = () => {
    // 表示正在加载
    loading.value = false
    const data = videoList.value?.slice((page - 1) * pageSize, page * pageSize) as VideoItem[]
    list.value.push(...data)
    // 页码累加
    page++
    if (videoList.value?.length === list.value.length) {
        finished.value = true
    }
}

// 初始化加载-主动请求前20条数据,用于首屏渲染,方便SEO
onLoad()
</script>

<style lang="scss" scoped>
// 视频列表
.video-list {
    display: flex;
    flex-wrap: wrap;
    padding: 10px 5px;
}
</style>