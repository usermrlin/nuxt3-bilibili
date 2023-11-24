<template>
    <div>
        <!-- 头部 -->
        <AppHeader />
        <!-- Sticky 粘性布局 -->
        <van-sticky>
            <!-- 弹幕 -->
            <van-barrage v-model="barrageList" :auto-play="false" ref="barrageRef">
                <!-- 视频 -->
                <video controls class="video-play" ref="videoRef" @play="onPlay" @pause="onPause"
                    :poster="imgURL + detail?.pic"
                    src="https://video.pearvideo.com/mp4/third/20230706/cont-1784445-12033417-151259-hd.mp4"></video>
            </van-barrage>
        </van-sticky>
        <!-- 标题作者信息 -->
        <div class="info">
            <h1 class="title-text">{{ detail?.title }}</h1>
            <div class="body">
                <div class="author">
                    <img class="avatar" :src="imgURL + detail?.author.face" />
                    <span class="name">{{ detail?.author.name }}</span>
                </div>
            </div>
        </div>
        <!-- 相关推荐 -->
        <div class="relate">
            <h3 class="relate-title">相关推荐</h3>
            <div class="relate-list">
                <!-- 视频列表 -->
                <van-list v-model:loading="loading" :finished="finished" finished-text="去 bilibili App 看更多" @load="onLoad">
                    <div class="video-list">
                        <AppVideo v-for="item in list" :key="item.bvid" :item="item"></AppVideo>
                    </div>
                </van-list>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import type { VideoItem } from '@/types/video'
// 引入vant弹幕组件
import type { BarrageInstance } from 'vant'
// 引入vue
import { ref } from 'vue'
// 引入nuxt
import { useFetch, useRoute } from 'nuxt/app';

const barrageRef = ref<BarrageInstance>()
// 防止图片加载不出来
let imgURL = ref('//images.weserv.nl/?url=')
// 弹幕相关
const barrageList = ref([
    { id: 100, text: '轻量' },
    { id: 101, text: '可定制的' },
    { id: 102, text: '移动端' },
    { id: 103, text: 'Vue' },
    { id: 104, text: '组件库' },
    { id: 105, text: 'VantUI' },
    { id: 106, text: '666' },
])
const onPlay = () => {
    barrageRef.value?.play()
}

const onPause = () => {
    barrageRef.value?.pause()
}

// 通过路由参数获取视频id
const { params } = useRoute()

const { data: detail } = useFetch(`/api/video/${params.id}`)

// 获取视频列表数据
const { data: videoList } = await useFetch('/api/video')
console.log(videoList)
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

</script>

<style lang="scss" scoped>
.video-list {
    display: flex;
    flex-wrap: wrap;
    padding: 10px 5px;
}

.video-play {
    width: 100vw;
    height: auto;
    object-fit: contain;
    background-color: #fff;
}

.info {
    padding: 10px;
    border-bottom: 1px solid #eee;

    .title-text {
        font-size: 16px;
        font-weight: normal;
    }

    .body {
        display: flex;
        margin-top: 20px;
        justify-content: space-between;
        align-items: center;
    }

    .author {
        display: flex;
        align-items: center;

        .avatar {
            width: 28px;
            height: 28px;
            border-radius: 50%;
            border: 1px solid #ccc;
        }

        .name {
            margin-left: 10px;
            font-size: 14px;
        }
    }
}

.relate {
    .relate-title {
        height: 32px;
        display: flex;
        align-items: center;
        font-size: 14px;
        font-weight: normal;
        padding: 0 10px;
        color: #333;
    }

    .relate-list {
        display: flex;
        flex-wrap: wrap;
        padding: 0 5px;
    }
}

// 视频列表
.video-list {
    display: flex;
    flex-wrap: wrap;
    padding: 10px 5px;
}
</style>