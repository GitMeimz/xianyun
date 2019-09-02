<template>
  <div class="tabs" style="margin-left:100px">
    <span v-for="(item,index) in ['新闻','体育','娱乐']" :key="index" @click="tabs(index)" :class="{active:curren === index}">{{item}}</span>

    <div>
      <ul v-show="curren === 0">
        <li>中秋将至</li>
        <li>中秋将至</li>
        <li>中秋将至</li>
      </ul>
    </div>
    <div>
      <ul v-show="curren === 1">
        <li>中秋将至11</li>
        <li>中秋将至11</li>
        <li>中秋将至11</li>
      </ul>
    </div>
    <div>
      <ul  v-show="curren === 2">
        <li>中秋将至22</li>
        <li>中秋将至22</li>
        <li>中秋将至22</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
    data(){
       return{
           curren:0
       }
    },
    methods:{
         tabs(index){
             this.curren=index
         }
    }
};
</script>

<style lang=less scoped>
   span{
       cursor: pointer;
       padding: 10px;
       
   }
   .active{
       color: red;
   }
</style>