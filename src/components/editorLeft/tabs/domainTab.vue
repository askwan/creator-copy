<template>
  <div class='domain-tab pd-big'>
    <h3 class="font-16 mg-bottom-big align-center">
      对象的时空参照
    </h3>
    <el-form size='mini' label-width="70px">
      <el-form-item label="时间参照">
        <el-input v-model="num"></el-input>
      </el-form-item>
      <el-form-item label="空间参照">
        <el-input v-model="num"></el-input>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
  export default {
    data(){
      return {
        num:''
      }
    },
    props:{},
    components:{},
    computed:{},
    mounted(){

    },
    methods:{

    }
  }
</script>
<style lang='scss' scoped>

</style>