# vue-spider-graph

## Install
``` bash
npm install vue-spider-graph --save
```

## Usage

#### Use in Vue Component

```html
<template>
  <div>
    <SpiderGraph  :height = "600"
                  :width = "600"
                  :numberOfDataPoints = "10"
                  :dataRatings = "[5,7,4,6,9,3,5,6,8,6]"
                  :predefinedRatings="[8,9,7,8,6,8,6,5,9,5]"
                  :predefinedRatingsColor = "`#F7A61B`"
                  :predefinedRatingsGridColor = "`rgba(247,166,27, 0.2)`"
                  :dataRatingsColor = "`#c40d1e`"
                  :dataRatingsColorGridColor = "`rgba(196,11,30, 0.2)`"></SpiderGraph>
  </div>
</template>
<script>
import SpiderGraph from 'vue-spider-graph'

new Vue({
  el: '#app',
  components: {
    SpiderGraph
  }
});
</script>
```
<br>
