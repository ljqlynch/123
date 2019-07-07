<template>
    <div class="containers">
        <el-row type="flex" justify="space-around">
            <!-- 侧边栏 -->
            <div class="aside">
                <div class="menus_body" @mouseleave="handleLeave">
                    <div class="menus">
                        <div class="menusitem" v-for="(item,index) in recommenList" :key="index" @mouseenter="handleCome(index)" :class="{active:currentNum == index}">
                            <span>{{item.type}}</span>
                        </div>
                    </div>

                    <div class="showMenus" v-if="showItem">
                        <ul>
                            <li v-for="(item,index) in cascadeList" :key="index">
                                <nuxt-link :to="`/post?city=${item.city}`">
                                    <i>{{index + 1}}</i>
                                    <strong>{{item.city}}</strong>
                                    <span>{{item.desc}}</span>
                                </nuxt-link>
                            </li>
                        </ul>
                    </div>
                </div>
                <div class="recommenCity">
                    <h4 class="recommenCity-title">推荐明星</h4>
                    <div class="recommenCity-item">
                        <nuxt-link to="#">
                            <img src="../../assets/923cb1b6c9fe29a2a2cbdd83f999c06e.gif" alt />
                        </nuxt-link>
                    </div>
                </div>
                <!-- 中间部分 -->
            </div>

            <div class=" content">
                <!-- 搜索框 -->
                <div class="wantgo">
                    <el-row type="flex" justify="space-between" align="middle" class="wantInput">
                        <!-- 推荐城市 -->
                        <input type="text" placeholder="请输入你想去的城市" v-model="searchCity" />
                        <i class="el-icon-search"></i>
                    </el-row>
                </div>
                <div class="search-recommend">
                    推荐:
                    <span v-for="(item,index) in [`广州`,`上海`,`成都`]" :key="index" @click="handleSearch(item)">
                        {{item}} </span>
                </div>
                <!-- 推荐攻略 -->
                <el-row type="flex" justify="space-between" align="middle" class="post-title">
                    <h4 class="strategy">旅游攻略</h4>
                    <el-button class="el-buttons">
                        <i class="el-icon-edit"></i>
                        <span>写游记</span>
                    </el-button>
                </el-row>
                <div class="postContent">
                    <!-- 筛选 -->

                    <Postinfo v-for="(item,index) in pageList" :key="index" :data ='item'/>

                    <!-- 分页 -->
                    <!-- size-change: 显示条数切换触发 -->
                    <!-- current-change: 切换页数时候触发 -->
                    <!-- current-page: 当前页数 -->
                    <!-- total: 总条数 -->
                    <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="pageIndex" :page-sizes="[3, 6, 9, 12]" :page-size="pageSize" layout="total, sizes, prev, pager, next, jumper" :total="total">
                    </el-pagination>

                </div>
            </div>
        </el-row>
    </div>
</template>

<script>
// import PostInfo from "@/components/post/postInfo.vue";
import Postinfo from '@/components/post/postInfo'
export default {

    components: {
        Postinfo
    },
    data() {

        return {

            //激活active
            currentNum: 100,
            showItem: false,
            //推荐城市
            recommenList: [],
            cascadeList: [],
            searchCity: "",

            //文章列表
            postInfo: [],
            //分页数据
            pageList: [],
            //分页
            pageIndex: 1, // 当前所在的页数
            pageSize: 3, //一页显示多少数据
            total: 0 //总数据
        };
    },
    methods: {
        //获取推荐城市
        getHotCity() {
            this.$axios({
                url: "/posts/cities"
            }).then(res => {
                const {
                    data
                } = res.data;
                this.recommenList = data;
            });
        },
        //获取攻略文章
        getPost() {
            this.$axios({
                url: '/posts'
            }).then(res => {
                console.log(res)
                const {
                    total,data
                } = res.data
                this.total = total;
                this.postInfo = data;
                this.pageList = data.slice(0,3)
            })
        },
        //当鼠标进入推荐城市时
        handleCome(index) {
            this.showItem = true;
            this.cascadeList = this.recommenList[index].children;
            this.currentNum = index;
        },
        //当鼠标离开推荐城市时
        handleLeave() {
            this.showItem = false;
            this.currentNum = 100;
        },
        //点击推荐
        handleSearch(value) {
            this.searchCity = value;
        },
        handleSizeChange(value) {
            this.pageSize = value;
            this.setDataList();
        },
        handleCurrentChange(value) {
            this.pageIndex = value;
            this.setDataList();
        },
        //显示分页数据
        setDataList() {
            this.pageList = this.postInfo.slice(
                (this.pageIndex-1) *pageSize,
                this.pageIndex *pageSize,

            );
        }
    },
    mounted() {
        //获取推荐城市
        this.getHotCity();
        this.getPost();
    }
};
</script>

<style lang="less">
.containers {
    width: 1000px;
    margin: 20px auto;
    padding: 20px 0;

    .aside {
        width: 260px;
        position: relative;
        z-index: 2;

        .menus_body {
            .menus {
                width: 260px;
                border: 1px solid #ddd;
                border-right: none;
                border-bottom: none;
                box-shadow: 0 0 1px #f5f5f5;
                z-index: 2;

                .menusitem {
                    height: 40px;
                    line-height: 40px;
                    border-bottom: 1px solid #ddd;
                    border-right: 1px solid #ddd;
                    padding: 0 20px;
                    font-size: 14px;
                    position: relative;
                    z-index: 2;

                    &:after {
                        display: block;
                        content: "";
                        width: 10px;
                        height: 10px;
                        border-right: 1px #999 solid;
                        border-top: 1px #999 solid;
                        transform: rotate(45deg);
                        position: absolute;
                        right: 20px;
                        top: 15px;
                    }

                    &.active {
                        border-right-color: #fff;
                        color: orange;

                        &:after {
                            border-right-color: orange;
                            border-top-color: orange;
                        }
                    }
                }
            }

            .showMenus {
                position: absolute;
                left: 260px;
                top: 0;
                width: 350px;
                padding: 10px 20px;
                box-sizing: border-box;
                background: #fff;
                border: 1px #ddd solid;

                ul li {
                    font-size: 14px;
                    line-height: 1.5;

                    * {
                        vertical-align: middle;
                    }

                    i {
                        color: orange;
                        font-size: 24px;
                        font-style: italic;
                    }

                    strong {
                        margin: 0 10px;
                        color: orange;
                        font-weight: normal;

                        &:hover {
                            text-decoration: underline;
                        }
                    }

                    span {
                        color: #999;

                        &:hover {
                            text-decoration: underline;
                        }
                    }
                }
            }
        }

        .recommenCity {
            margin-top: 20px;

            .recommenCity-title {
                font-weight: normal;
                padding-bottom: 10px;
                border-bottom: 1px #ddd solid;
                margin-bottom: 10px;
            }

            .recommenCity-item {
                img {
                    width: 100%;
                    display: block;
                }
            }
        }
    }

    .content {
        width: 700px;

        .wantgo {
            width: 100%;
            box-sizing: border-box;
            height: 40px;
            line-height: 40px;
            border: 3px orange solid;

            input {
                flex: 1;
                padding: 0 20px;
                line-height: 40px;
                outline: none;
                border: none;
                background: none;
            }

            i {
                font-size: 24px;
                color: orange;
                font-weight: bold;
                margin-right: 10px;
            }
        }

        .search-recommend {
            padding: 10px 0;
            font-size: 12px;
            color: #666;

            span {
                margin-right: 5px;

                &:hover {
                    color: orange;
                    cursor: pointer;
                }
            }
        }

        .post-title {
            padding-bottom: 10px;
            border-bottom: 1px solid #eee;
            position: relative;

            .strategy {
                font-weight: 400;
                font-size: 18px;
                color: orange;

                &::after {
                    display: block;
                    content: "";
                    width: 72px;
                    height: 2px;
                    background: orange;
                    position: absolute;
                    bottom: 0;
                    left: 0;
                }
            }

            .el-buttons {
                color: #fff;
                background-color: #409eff;
                border-color: #409eff;
            }
        }
    }
}
</style>
