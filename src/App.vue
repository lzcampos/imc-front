<template>
  <imc />
</template>

<script>
import imc from "./components/imc.vue";

export default {
  name: "App",
  components: {
    imc,
  },
};
</script>

<style lang="scss">
html,
body {
  margin: 0px;
  height: 100%;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
