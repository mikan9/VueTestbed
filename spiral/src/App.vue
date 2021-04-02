<template>
  <Spiral :count="6" />
</template>

<script>
import Spiral from "./components/Spiral.vue";

export default {
  name: "App",
  components: {
    Spiral,
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
