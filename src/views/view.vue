<template>
  <div class='view fill'>
    <div class="search">
      <search-bar @startSearch="search" :searchValue="viewSearchValue"></search-bar>
    </div>
    <div id="mapbox" class="fill"></div>
    <div class="view-left" v-show="showLeft">
      <component :is="componentId" :viewSearchValue="viewSearchValue" :sobject="currentObject"></component>
    </div>
  </div>
</template>
<script>
  import {vm,operate} from '@/script/operate'
  import * as mapbox from '@/script/mapbox'
  import psde from '@/script/editor/psde'
  import mapposition from '@/script/mapposition'
  let map;
  export default {
    data(){
      return {
        currentObject:{},
        showLeft:false,
        componentId:'',
        viewSearchValue:""
      }
    },
    props:{},
    components:{
      'viewLeft':()=>import('@/components/viewLeft/objDetail'),
      'searchBar':()=>import('@/components/common/searchBar'),
      'historyList':()=>import('@/components/viewLeft/historyList'),
      'viewExport':()=>import('@/components/viewLeft/viewExport'),
      'searchResult':()=>import('@/components/viewLeft/searchResult')
    },
    computed:{},
    watch:{
      'currentObject.id'(val){
        // this.showLeft = Boolean(val);
      }
    },
    mounted(){
      map = mapbox.createMapboxMap('mapbox',()=>{
        //定位
        let position = mapposition.getMapPosition();
        map.setCenter([position.lng, position.lat], position.zoom );
        map.setZoom(position.zoom);
      });
      this.listenEvent();
    },
    methods:{
      listenEvent(){
        vm.$on(operate.selectObject,obj=>{
          if(!obj.id) {
            this.currentObject = {};
            this.showLeft = false;
            this.viewSearchValue = '';
            return
          }
          psde.objectQuery.getDetailById.query({ids:obj.id}).then(res=>{
            this.currentObject = res.list[0];
            this.componentId = 'viewLeft';
            this.showLeft = true;
          })
        });
        vm.$on(operate.openTab,obj=>{
          this.componentId = obj.name;
          this.showLeft = true;
        })
      },
      search(val){
        this.viewSearchValue = val;
        this.componentId = 'searchResult'
        this.showLeft = true;
      }
    },
    destroyed(){
      vm.$off([operate.selectObject]);
      
      mapposition.saveMapPosition({
        lng: map.getCenter().lng,
        lat: map.getCenter().lat,
        zoom: map.getZoom() 
      });
    }
  }
</script>
<style lang='scss' scoped>
  .view{
    // background-color: red;
    .search{
      position: absolute;
      left:20px;
      top: 10px;
      width: 300px;
      z-index: 10;
    }
    .view-left{
      position: absolute;
      background-color: #fff;
      left: 20px;
      top: 60px;
      height: 80%;
      width: 300px;
      overflow-y: auto;
      overflow-x: hidden;
    }
  }
</style>