<template>
    <section class="contianer">
        <div>
         <Hotelheader/>
        </div>
        <div>
        </div>
   </section>
</template>
<script>
import Hotelheader from "@/components/hotel/hotelheader.vue"
export default {
    components:{
        Hotelheader
    }
}
</script>
<style lang="less" scoped>
   .contianer{
        width:1000px;
        margin:20px auto;
    }
 
</style>
