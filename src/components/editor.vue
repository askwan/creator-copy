<template>
  <div class='editor-box fill flex'>
    <div class="left">
      <left-content :sobject="currentObj" :entity='entity'></left-content>
    </div>
    <div class="fill" id="container"></div>
  </div>
</template>
<script>
  import Editor from '@/script/editor';
  import {vm,operate,getEditor} from '@/script/operate'
  import {State,findOtypeById} from '@/script/editor/utils/store'
  let editor
  export default {
    data(){
      return {
        currentObj:{},
        entity:{}
      }
    },
    props:{},
    components:{
      'left-content':()=>import('./editorLeft')
    },
    computed:{},
    mounted(){
      this.$nextTick(()=>{
        this.initIdEditor();
      })
    },
    methods:{
      initIdEditor(){
        editor = new Editor();
        editor.init(document.getElementById('container'),context=>{
          console.log('ready');
          editor.on('currentObject',data=>{
            if(data.object) {
              this.currentObj = data.object;
              vm.$emit(operate.changeTab,{name:'objectDetail'});
            }else if(data.entityId){
              this.entity = context.entity(data.entityId);
              vm.$emit(operate.changeTab,{name:'diagramList'});
            }else if(!data.entityId){
              vm.$emit(operate.changeTab,{name:''});
            }
          })
        });
        getEditor(editor);
      }
    }
  }
</script>
<style lang='scss' scoped>
  .left{
    width: 300px;
    height: 100%;
    flex-shrink: 0;
  }
</style>