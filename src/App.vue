<template>
  <div id="q-app">
  <!-- <div class="top">
    <span>德弘云组态</span>
  </div> -->
    <router-view />
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
/* .top{
  width: 100%; height: 20px; margin: 0 auto;
  } */
::-webkit-scrollbar {
  /* 滚动条整体样式 */
  width: 3px;
  height: 3px;
}
::-webkit-scrollbar-thumb {
  /* 滚动条内部样式 */
  background-color: rgb(0, 240, 255);
}
::-webkit-scrollbar-track {
  /* 滚动条容器样式 */
  background-color: rgba(79, 116, 238, 0.719);
}
</style>
