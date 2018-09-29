<template>
  <div class='object-list pd-big'>
    <div v-for="item in objectList" :key="item.id" class="object-el pointer-shadow radius-2 mg-bottom-big flex-align pd-left-small pd-right-small" @click="selectObj(item)">
      <div class="icon-text flex-center radius-2 mg-right-big">
        <span class="font-14 font-white">{{item.name|splice2}}</span>
      </div>
      <div class="el-content">
        <p class="font-black font-14 text-ellipsis">{{item.name|formatstr}}（ {{item.otype.id|toName}} ）</p>
        <p class="font-info font-12 text-ellipsis" :title="item.attributes|toString">属性：{{item.attributes|toString}}</p>
        <div class="align-right"><span class="pointer-danger font-info" @click.stop="deleteObj(item)">删除</span></div>
      </div>
    </div>
    <div class="block">
      <el-pagination
        layout="prev, pager, next"
        small
        :total="1000">
      </el-pagination>
    </div>
  </div>
</template>
<script>
  import psde from '@/script/editor/psde'
  import {State} from '@/script/editor/utils/store'
  import {vm,operate,getEditor} from '@/script/operate'
  let IdEdit;
  export default {
    data(){
      return {
        objectList:[]
      }
    },
    props:{},
    components:{},
    computed:{

    },
    filters:{
      splice2(str){
        if(typeof str === 'string' &&str.length>0){
          return str.slice(0,2);
        }else{
          return 'default'
        }
      },
      toString(arr){
        let _arr = arr.map(el=>el.name);
        return _arr.join(';')
      },
      toName(id){
        // console.log(State.otypes,id);
        let otype = State.otypes[id];
        return (typeof otype === 'object' && otype.name.length>0) ? otype.name : 'default'
      },
      formatstr(str){
        return str.length>0?str:'defalut'
      }
    },
    mounted(){
      this.queryObject();
    },
    methods:{
      queryObject(){
        var obj = {
					names: '',
					otNames: '',
					pageNum: 1,
					pageSize: 20
				};
        psde.objectQuery.ByNameAndOTName.query(obj).then(res => {
          this.objectList = res.list;
				});
      },
      selectObj(obj){
        IdEdit = getEditor();
        IdEdit.updateParent(obj);
        vm.$emit(operate.changeTab,{name:'objectDetail'});
      },
      deleteObj(obj){
        console.log("delete",obj)
      }
    }
  }
</script>
<style lang='scss' scoped>
  .object-list{
    .object-el{
      background-color: #fff;
      height: 80px;
      border: 1px solid #ccc;
      .icon-text{
        width: 50px;
        height: 50px;
        background: #b2d2ff;
        flex-shrink: 0;
      }
      .el-content{
        flex-grow: 1;
        p{
          width: 150px;
        }
      }
    }
  }
</style>