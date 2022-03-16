<template>
  <ExampleOne />
  <ExampleTwo />
</template>

<script>
import ExampleOne from "./components/ExampleOne.vue";
import ExampleTwo from "./components/ExampleTwo.vue";

export default {
  name: "App",
  components: {
    ExampleOne,
    ExampleTwo,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
