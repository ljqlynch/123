<template>
    <div class="container">
        <!-- <div class="container">
        <div class="block">
            <span class="demonstration">hover 触发子菜单</span>
            <el-cascader
                v-model="value"
                :options="options"
                :props="{ expandTrigger: 'hover' }"
                @change="handleChange"
            ></el-cascader>
        </div> -->
        <div class="menus-body">
            <table border="1" cellspacing="0" v-for="(item,index) in data " :key="index">
                <tr class="tableClss">
                    <td>{{item.type}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;></td>
                </tr>
            </table>
        </div>
        <div class="yincanbox">
            
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            value: [],
            data: [{
                type: '',
                children: []
            }, ]
        };
    },
    mounted() {
        //刷新获取菜单
        this.handleCityInfo()
    },
    methods: {
        handleCityInfo() {
            this.$axios({
                url: "/posts/cities",
            }).then(res => {
                console.log(res)
                this.data = res.data.data
            })
        },

        handleChange(value) {
            // console.log(value);
        }
    }
};
</script>

<style lang="less" scoped>
.container {
    width: 1000px;
    margin: 0 auto;
    position: relative;

    .tableClss {
        line-height: 25px
    }
}
</style>
