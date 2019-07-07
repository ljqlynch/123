<template>
    <!-- 思路:当图片长度超多三张,显示左右左右排列,否则上下排列 -->
    <!-- 左右排列 -->
    <el-row type="flex" justify="space-betwwen" align="midedle" class="post-item">
        <!-- 左边图片 -->
        <el-row type="flex" align="middle" class="post-cover">
            <img :src="data.images[0]">
        </el-row>
        <div class="post-content">
            <h4 :title="data.title" class="post-title">
                {{data.title}}
            </h4>
            <p>{{data.summary}}</p>
            <div>
                <span>{{data.cityName}}</span>
                <span>{{data.account.nickname}}</span>
                <span>{{data.watch || 0}}</span>
                <span class="post-info-right">{{data.like || 0}}赞</span>
            </div>
        </div>
    </el-row>
</template>

<script>
export default {
    props: {
        data: {
            type: Object,
        },
    },

    data() {
        return {

        }
    }
}
</script>

<style lang="less">
.post-item {
    width: 100%;
    padding: 20px 0;
    border-bottom: 1px #eee solid
}

.image-text {
    .post-content {
        width: 470px;
        p {
            color: #666666
        }
    }

    .post-desc {}
}

.post-cover {
    width: 220px;
    height: 150px;
    overflow: hidden;
    flex-shrink: 0;
    margin-right: 10px;

    img {
        display: block;
        width: 100%;
        height: 100%;
        object-fit: cover;
    }
}
</style>
