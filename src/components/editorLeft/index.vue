<template>
  <div class='left-box fill'>
    <div class="object-title pd-left-small pd-right-small">
      <span class="font-16 text-ellipsis">Name is default ( 999 ) 吴浩</span>
    </div>
    <div class="object-detail">
      <!-- <div v-for="i in 100" :key="i">{{i}}</div> -->
      <keep-alive>
        <component :is="componentId" :currentObject="sobject" :entity="entity"></component>
      </keep-alive>
    </div>
  </div>
</template>
<script>
  import {vm,operate} from '@/script/operate'
  export default {
    data(){
      return {
        componentId:''
      }
    },
    props:['sobject','entity'],
    components:{
      'diagramList':()=>import('./tabs/diagramList.vue'),
      'objectDetail':()=>import('./tabs/objectDetail.vue'),
      'relationOperate':()=>import('./tabs/relationOperate.vue'),
      'objectList':()=>import('./tabs/objectList.vue'),
      'relationList':()=>import('./tabs/relationList.vue')
    },
    computed:{},
    mounted(){
      this.listenEvent();
    },
    methods:{
      listenEvent(){
        vm.$on(operate.changeTab,obj=>{
          this.componentId = obj.name;
        })
      }
    }
  }
</script>
<style lang='scss' scoped>
  $height:50px;
  .left-box{
    .object-title{
      height: $height;
      border-bottom: 1px solid #ccc;
      span{
        line-height: $height;
      }
    }
    .object-detail{
      position:absolute;
      left:0;
      right:0;
      top:$height;
      bottom:0;
      overflow-y: auto;
      overflow-x: hidden;
    }
  }
</style>