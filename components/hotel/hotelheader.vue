<template>
  <div class="contianer">
      <div class="header">
      <el-autocomplete
      :fetch-suggestions="hotelDepartSearch"
      placeholder="目的地"
      @select="handleDepartSelect"
      class="el-autocomplete"
      v-model="form.departCity"
    ></el-autocomplete>
    
    <div class="block">
    <span class="demonstration" style="margin:5px"></span>
    <el-date-picker
      v-model="value1"
      type="daterange"
      range-separator="至"
      start-placeholder="开始日期"
      end-placeholder="结束日期">
    </el-date-picker>
    </div>
    <div style="margin:5px"></div>
   <el-select v-model="value" placeholder="请输入人数">
    <!-- <el-option class=""></el-option>
    <el-option></el-option> --> -->
       <!-- v-for="item in options"
      :key="item.value"
      :label="item.label"
      :value="item.value"> -->
  </el-select>
  <div style="margin:5px"></div>
  <el-button type="primary" icon="el-icon-search" @click="hendlePrice">查看价格</el-button>
  </div>
  <div>
       <span>区域：</span> 
       <div>
           <a href="">全部</a>
       </div>
  </div>
  </div>
</template> 

<script>
export default {
    data(){
        return{
          form:{
              departCity:""
          },
            pickerOptions: {
          shortcuts: [{
            text: '最近一周',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
              picker.$emit('pick', [start, end]);
            }
          }, {
            text: '最近一个月',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
              picker.$emit('pick', [start, end]);
            }
          }, {
            text: '最近三个月',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
              picker.$emit('pick', [start, end]);
            }
          }]
        },
        handleDepartSelect:'',
        value1: '',
        value2: '',
        //  options: [{
        //   value: '选项1',
        //   label: '黄金糕'
        // }, {
        //   value: '选项2',
        //   label: '双皮奶'
        // }, {
        //   value: '选项3',
        //   label: '蚵仔煎'
        // }, {
        //   value: '选项4',
        //   label: '龙须面'
        // }, {
        //   value: '选项5',
        //   label: '北京烤鸭'
        // }],
        value: '一个孤儿'
      };
},
    methods:{
            hendlePrice(){
            
            },
            hotelDepartSearch(value, cb){
            if(!value) return;

            // 请求机票城市的接口
            this.$axios({
                url: "/airs/city?name=" + value,
                method: "GET",
            }).then(res => {
                const {data} = res.data;

                // 给data每一项都加value
                const newData = data.map(v => {
                    return {
                        ...v,
                        value: v.name.replace("市", "")
                    }
                })

                // 默认选中第一项
                this.form.departCode = newData[0].sort;
                this.form.departCity = newData[0].value;

                cb(newData);
            })
        },
    }
};
</script>
<style lang="less" scoped>
.contianer {
  width: 1000px;
  margin: 20px auto;
  .header{
      display: flex;
      
  }
}
</style>
