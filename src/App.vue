<template>
  <div id="app" class="fill">
    <nav class="header-box"></nav>
    <div class="map-content">
      <router-view/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style scoped lang='scss'>
  $height:53px;
  .header-box{
    height: $height;
    border-bottom: 1px solid #ccc;
  }
  .map-content{
    position: absolute;
    left:0;
    right: 0;
    top: $height;
    bottom:0;
  }
</style>
