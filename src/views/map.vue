<template>
  <div class='map fill'>
    <map-content></map-content>
  </div>
</template>
<script>
  import psde from '@/script/editor/psde';
  import {State,getDiagram} from '@/script/editor/utils/store'
  import axios from 'axios'
  import { queryModelFile, downloadFile } from "@/script/editor/psde/config";
  import {vm,operate} from '@/script/operate'
  export default {
    data(){
      return {

      }
    },
    props:{},
    components:{
      'map-content': () => import('../components/editor')
    },
    computed:{},
    mounted(){
      this.getDiagram();
      this.getModels();
      this.getRelationType();
    }, 
    methods:{
      getDiagram(){
        new psde.Diagram()
        .query({
          loadField: true,
          loadModel: true,
          loadForm: true,
          loadParentField: true,
          loadParents: true,
          loadConnector: true
        })
        .then(res => {
          getDiagram(res.list);
        }).catch(err=>{
          vm.$emit(operate.notice,{
            type:'error',
            title:'网络错误',
            message:err.message
          })
        })
      },
      getModels(){
        
        axios.get(queryModelFile.baseURL, {})
          .then(response => {
            State.ModelList = response.data.data.list;
          })
          .catch(function(error) {
            vm.$emit(operate.notice,{
              type:'error',
              title:'网络错误',
              message:error.message
            })
          });
      },
      getRelationType(){
        new psde.GetDict().query(null,'relation').then(res=>{
          State.relationType = res;
        }).catch(err=>{
          vm.$emit(operate.notice,{
            type:'error',
            title:'网络错误',
            message:error.message
          })
        })
      }
    }
  }
</script>
<style scoped lang="scss">

</style>

