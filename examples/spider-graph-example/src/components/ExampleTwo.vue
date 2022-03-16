<template>
  <div>
    <SpiderGraph
      ref="spiderGraphRef"
      :height="600"
      :width="600"
      :numberOfDataPoints="10"
      :dataRatings="this.dataRatings"
      :predefinedRatings="[8, 9, 7, 8, 6, 8, 6, 5, 9, 5]"
      :predefinedRatingsColor="`#F7A61B`"
      :predefinedRatingsGridColor="`rgba(247,166,27, 0.2)`"
      :dataRatingsColor="`#c40d1e`"
      :dataRatingsColorGridColor="`rgba(196,11,30, 0.2)`"
    ></SpiderGraph>
  </div>
</template>
<script>
import SpiderGraph from "vue-spider-graph";

export default {
  name: "ExampleTwo",
  components: {
    SpiderGraph,
  },
  created() {
    this.dataRatings = [5, 7, 4, 6, 9, 3, 5, 6, 8, 6];
  },
  mounted() {
    //I am Changing value of first Index by 1 every second and redrwaing Graph
    let spiderGraph = this.$refs["spiderGraphRef"];
    setInterval(() => {
      if (this.dataRatings[0] == 10) this.dataRatings[0] = 0;
      this.dataRatings[0]++;
      spiderGraph.draw();
    }, 1000);
  },
};
</script>