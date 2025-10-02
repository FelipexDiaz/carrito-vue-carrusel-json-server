<template>
  <div id="app">
    <CatalogoProductos />
  </div>
</template>

<script>
import CatalogoProductos from "./components/CatalogoProductos.vue";

export default {
  name: "App",
  components: { CatalogoProductos }
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background: #fafafa;
}
#app {
  max-width: 900px;
  margin: auto;
  padding: 2rem;
}
</style>
