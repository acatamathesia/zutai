<template>
  <div id="mydemo" class="topo-fullscreen">
    <TopoRender />
    <!-- <button @click="downLoad">点击下载</button> -->
  </div>
</template>

<script>
import TopoRender from "@/components/topo/TopoRender";
import html2canvas from "html2canvas";
export default {
  name: "TopoFullscreen",
  components: {
    TopoRender
  },
  methods: {
    downLoad() {
      html2canvas(document.getElementsByClassName('topo-render')[0]).then(function(canvas) {
        document.body.appendChild(canvas)
        console.log(url)
      });
    }
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


