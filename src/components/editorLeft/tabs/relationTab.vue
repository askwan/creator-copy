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
        <div class="relation-list">
          <el-collapse>
            <el-collapse-item v-for='node in nodes' :key="node.id">
              <template slot="title">
                <div class="relation-el flex-align">
                  <span class="font-14">{{objectDetail.name|formateName}}</span>
                  <div class="flex-column">
                    <span class="r-name">{{node.edge.relation.name}}</span>
                    <div class="flex-center underline-box">
                      <span class="underline"></span>
                      <i class="iconfont icon-zuojiantou"></i>
                    </div>
                  </div>
                  <span class="font-14">{{node.label}}</span>
                  <i class="el-icon-delete mg-left-big pointer font-danger font-16"></i>
                  <!-- {{objectDetail.name}}---{{node.edge.relation.name}} -----{{node.label}} -->
                </div>
              </template>
            </el-collapse-item>
          </el-collapse>

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
        nodes:[]
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
    filters:{
      formateName(str){
        return str?str:'defalut'
      }
    },
    watch:{
      objectDetail(){
        this.parents = this.objectDetail.parents;
        this.nodes = this.objectDetail.network.nodes;
        console.log(this.parents,999696)
      }
    },
    mounted(){
      this.parents = this.objectDetail.parents;
      this.nodes = this.objectDetail.network.nodes;
    },
    activated(){
      this.parents = this.objectDetail.parents;
      this.nodes = this.objectDetail.network.nodes;
      console.log(78979)
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
    .relation-list{
      .relation-el{
        .r-name{
          margin-bottom: -5px;
        }
        .underline-box{
          margin: -35px 0px 0px; 
          i{
            display: inline-block;
            margin-left: -5px;
            padding-top: 1px;
          }
        }
        
      }
      .underline{
        display: inline-block;
        width: 80px;
        border: 0.5px solid #999;
        
      }
    }
  }

</style>