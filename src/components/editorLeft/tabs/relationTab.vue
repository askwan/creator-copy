<template>
  <div class='relation-box pd-left-big pd-right-big'>
    <el-collapse v-model="activeNames">
      <el-collapse-item title="父对象" name="1">
        <div class="add-content pointer-default flex-center mg-bottom-big" @click="chooseParent">
          <span class="font-14 no-select">+选择父对象</span>
        </div>
        <div class="">
          <el-tag v-for="parent in parents" class="mg-right-small mg-bottom-small" :key="parent" @close="deleteParent(parent)" closable disable-transitions>{{parent}}</el-tag>
        </div>
      </el-collapse-item>
      <el-collapse-item title="关系对象" name="2">
        <div class="add-content pointer-default flex-center mg-bottom-big" @click="chooseRelation">
          <span class="font-14 no-select">+选择关系对象</span>
        </div>
        <div class="">
          <el-tag class="mg-right-small mg-bottom-small" v-for="i in 5" :key="i" @close="deleteParent" closable disable-transitions>地球</el-tag>
        </div>
      </el-collapse-item>
    </el-collapse>
  </div>
</template>
<script>
  import {vm,operate,getEditor} from '@/script/operate';
  export default {
    data(){
      return {
        activeNames:['1'],
        parents:[],
      }
    },
    props:{
      objectDetail:{
        type:Object,
        defalut:{}
      }
    },
    components:{},
    computed:{},
    mounted(){
      console.log('mounted',this.parents);
      this.parents = this.objectDetail.parents;
      this.nodes = this.objectDetail.network.nodes;
    },
    activated(){
      this.sobject = this.objectDetail;
      this.parents = this.objectDetail.parents;
      this.nodes = this.objectDetail.network.nodes;
      console.log(this.parents,555555)
    },
    methods:{
      deleteParent(parent){
        console.log('deleteParent');
        getEditor().deleteParent(parent);
        this.parents = this.parents.filter(el=>el!=parent);
      },
      deleteRelation(){
        console.log('deleteRelation');
      },
      chooseParent(){
        vm.$emit(operate.changeTab,{name:'objectList'});
      },
      chooseRelation(){
        vm.$emit(operate.changeTab,{name:'relationList'});
      }
    }
  }
</script>
<style lang='scss' scoped>
  .relation-box{
    .add-content{
      height: 70px;
      background-color: #f5f7fa;
      border: 2px dashed #e1e3e6;
      border-radius: 4px;
    }
  }
</style>