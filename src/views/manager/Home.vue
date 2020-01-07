<template>
  <div class="home">
    <!-- 头部 -->
    <header class="header">
      <img src="../../assets/home.jpg" alt="">
    </header>
    <!-- 内容 -->
   <div>
     <!-- 分类 -->
    <van-grid :column-num="3">
    <van-grid-item
    v-for="value in categories"
    :key="value.id"
    :icon="value.icon"
    :text="value.name"
  />
    </van-grid>
    <briup-product-item 
      @click="toBuyHandler(p)"
      v-for="p in products" 
      :key="p.id" 
      :data="p">
    </briup-product-item> 
   </div>
  </div>
</template>
<script>
import {get,post} from '../../http/axios';
export default {
  data(){
    return{
      categories:[],
      products:[]
    }
  },
  created(){
    // 查询栏目信息
    this.loadCategories();
    // 查询产品
    this.loadProducts();
  },
  methods:{
     toBuyHandler(p){
      //  跳转到确认订单页面，并携带数据p
        this.$router.push({
        path:"/manager/order_confirm",
        query:p
      })

    },
    //加载栏目信息
    loadCategories(){
      let url="/category/findAll";
      get(url).then((response)=>{
        //将查询结果控制到前六个
        this.categories=response.data.slice(0,6);
      })

    },
    loadProducts(){
      let url="/product/query"
      let params={
        page:0,
        pageSize:10
      }
      post(url,params).then((response)=>{
        this.products=response.data.list;
      })
    }
  }
}
</script>
<style scoped>
.home {
  padding-bottom: 50px;
}
.header {
  height: 300px;
  overflow: hidden;
}
.header img {
  width: 100%;
}
</style>