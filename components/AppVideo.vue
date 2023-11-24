<template>
    <NuxtLink class="v-card" :to="`/video/${item.bvid}`">
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
</template>

<script setup lang="ts">
import type { VideoItem } from '@/types/video'
import { defineProps } from 'vue'
defineProps<{
    item: VideoItem
}>()
// 防止图片加载不出来
let imgURL = ref('//images.weserv.nl/?url=')
</script>

<style lang="scss" scoped>
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