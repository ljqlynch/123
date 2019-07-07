<template>
    <!-- 思路:当图片长度超多三张,显示左右左右排列,否则上下排列 -->
    <!-- 左右排列 -->
    <div>
        <el-row type="flex" justify="space-between" align="midedle" class="post-item" v-if="showImg === `sidebar`">
            <!-- 左边图片 -->
            <el-row type="flex" align="middle" class="post-cover">
                <img :src="data.images[0]">
            </el-row>
            <div class="post-content">
                <h4 :title="data.title" class="post-title">
                    {{data.title}}
                </h4>
                <p class="data-summary">{{data.summary}}</p>

                <div>
                    <el-row type="flex" justify="space-betwent">
                        <span class="data-account-defaultAvatar">
                            <img :src="$axios.defaults.baseURL+data.account.defaultAvatar">
                        </span>
                        <span>{{data.cityName}}</span>
                        <span class="data-account-nickname">{{data.account.nickname}}</span>
                        <span>{{data.watch || 0}}</span>

                        <span class="post-info-right">{{data.like || 0}}赞</span>
                    </el-row>
                </div>
            </div>
        </el-row>

        <!-- 底部排列 -->
        <el-row type="flex" justify="space-between" align="middle"></el-row>
    </div>
</template>

<script>
export default {
    props: {
        data: {
            type: Object
        }
    },

    data() {
        return {
            showImg: `sidebar`
        };
    }
};
</script>

<style lang="less">
.post-item {
    width: 100%;
    padding: 20px 0;
    border-bottom: 1px #eee solid;
}

.post-content {
    width: 470px;

    .data-summary {
        color: #666666;
        overflow: hidden;
        height: 101px;
    }

    .data-account-nickname {
        color: orange;
    }

    img {
        width: 30px;
        height: 30px;
        border-right: 100%;
        margin: 5px;
    }
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
