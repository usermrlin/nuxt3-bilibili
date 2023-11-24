<template>
    <div class="search">
        <header class="search-header">
            <van-search show-action color="red" v-model="value" placeholder="请输入搜索关键词" @search="onSearch"
                @cancel="onCancel" />
        </header>
        <div class="we">
            <h2>大家都在搜</h2>
            <ul>
                <li v-for="item in list" :key="item.id">{{ item.content }}</li>
            </ul>
        </div>
        <div class="history">
            <h2>历史搜索</h2>
            <ul>
                <li v-for="item in list_history" :key="item.id">
                    <van-icon name="clock-o" class="ls" />
                    <nuxt-link to="javascript:void(0)">{{ item.content }}</nuxt-link>
                </li>
            </ul>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'
let value = ref('')
let list = ref([
    {
        id: 1,
        content: '中方将对6国试行免签政策'
    },
    {
        id: 2,
        content: '元气骑士前传致歉'
    },
    {
        id: 3,
        content: 'S赛历届夺冠队伍胜率'
    }
])
let list_history = ref([
    {
        id: 1,
        content: '王者荣耀'
    },
    {
        id: 2,
        content: 'bilibili'
    }
])


const onSearch = (val) => {
    showToast(val)
}
const onCancel = () => {
    showToast('取消')
}
</script>

<style lang="scss" scoped>
$text_color: #999;

.search {
    background-color: #f4f4f4;
}

.we {
    padding: 10px;
    background-color: #fff;

    h2 {
        color: $text_color;
        font-size: 14px;
    }

    ul {
        display: grid;
        grid-template-columns: 1fr 1fr;
        margin-top: 10px;
        padding-bottom: 20px;

        li {
            border: 1px solid $text_color;
            margin: 8px 8px 0 0;
            padding: 8px;
            border-radius: 18px;
            font-size: 14px;
            white-space: nowrap;
            color: #666;


        }
    }
}

.history {
    margin-top: 10px;
    background-color: #fff;
    padding: 10px;

    h2 {
        color: $text_color;
        font-size: 14px;
    }

    ul {
        li {
            height: 50px;
            line-height: 50px;
            border-bottom: 1px solid #ccc;

            .ls {
                margin-right: 10px;
            }

            a {
                color: #050505;
            }
        }
    }
}
</style>