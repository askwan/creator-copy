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
        });
      },
      getModels(){
        axios.get(queryModelFile.baseURL, {})
          .then(response => {
            State.ModelList = response.data.data.list;
          })
          .catch(function(error) {

          });
      }
      
    }
  }
</script>
<style lang='scss' scoped>

</style>