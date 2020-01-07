<template>
  <div id="mydemo" class="topo-fullscreen">
    <TopoRender />
  </div>
</template>

<script>
import TopoRender from "@/components/topo/TopoRender";
export default {
  name: "TopoFullscreen",
  components: {
    TopoRender
  },
  created() {},
  mounted() {}
};
</script>

<style lang="scss">
body{
  width: 100%;
  height: 100%;
}
.topo-fullscreen {
  width: 100%;
  height: 100%;
}
</style>


