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
</template>
<script setup lang="ts">
// 引入Api
// 防止图片加载不出来
let imgURL = ref('//images.weserv.nl/?url=')
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