<template>
  <div class='detail pd-mini'>
    <el-collapse v-model='activeNames'>
      <el-collapse-item title="属性" name="1">
        <div class="tab-box">
          <property-tab :objectDetail="sobject"></property-tab>
        </div>
      </el-collapse-item>
      <el-collapse-item title="形态" name="2">
        <div class="tab-box">
          <form-tab :objectDetail="sobject"></form-tab>
        </div>
      </el-collapse-item>
      <el-collapse-item title="关系" name="3">
        <div class="tab-box">
          <relation-tab :objectDetail="sobject"></relation-tab>
        </div>
      </el-collapse-item>
      <el-collapse-item title="行为" name="4">
        <div class="tab-box">
        </div>
      </el-collapse-item>
      <el-collapse-item title="时空" name="5">
        <div class="tab-box"></div>
      </el-collapse-item>
    </el-collapse>
  </div>
</template>
<script>
  import {State,findOtypeById} from '@/script/editor/utils/store'
  export default {
    data(){
      return {
        sobject:{},
        otype:{},
        activeNames: ['3'],
      }
    },
    props:{
      currentObject:{
        default:{},
        type:Object
      }
    },
    computed:{

    },
    watch:{
      currentObject(obj){
        this.sobject = obj;
      }
    },
    components:{
      'propertyTab':()=>import('./propertyTab.vue'),
      'formTab':()=>import('./formTab.vue'),
      'relationTab':()=>import('./relationTab.vue')
    },
    computed:{},
    activated(){
      this.sobject = this.currentObject;
      console.log(this.sobject);
    },
    methods:{
      
    }
  }
</script>
<style lang='scss' scoped>

</style>