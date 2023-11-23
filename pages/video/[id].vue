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
                        <!-- <AppVideo v-for="item in list" :key="item.bvid" :item="item"></AppVideo> -->
                        <NuxtLink class="v-card" v-for="item in videoList" :key="item.aid" :to="`/video/${item.bvid}`">
                            <div class="card">
                                <div class="card-img">
                                    <img class="pic" :src="imgURL + item.pic" :alt="item.title" />
                                </div>
                                <div class="count">
                                    <span>
                                        <i class="iconfont icon_shipin_bofangshu"></i>
                                        {{ item.stat.view }}
                                    </span>
                                    <span>
                                        <i class="iconfont icon_shipin_danmushu"></i>
                                        {{ item.stat.danmaku }}
                                    </span>
                                </div>
                            </div>
                            <p class="title">{{ item.title }}</p>
                        </NuxtLink>
                    </div>
                </van-list>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
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
console.log(detail);
// 获取视频列表数据
const { data: videoList } = useFetch('/api/video')
console.log(videoList);
</script>

<style lang="scss" scoped>
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

// 视频卡片
.v-card {
    width: 50% !important;
    padding: 0 5px 10px;

    .card {
        position: relative;
        background: #f3f3f3 url(@/assets/images/default.png) center no-repeat;
        background-size: 36%;
        border-radius: 2px;
        overflow: hidden;

        .card-img {
            .pic {
                height: 100px;
                width: 100%;
                object-fit: cover;
            }
        }

        .count {
            background-image: linear-gradient(0deg, #000000d9, #0000);
            color: #fff;
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            font-size: 12px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 5px 6px;

            span {
                .iconfont {
                    font-size: 12px;
                }
            }
        }
    }

    .title {
        margin-top: 5px;
        font-size: 12px;
        color: #212121;
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
    }
}
</style>