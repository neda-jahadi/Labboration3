<template>
  <div id="app">
    <DataComponent />

  </div>
</template>

<script>
import  DataComponent  from "./components/DataComponent.vue";

export default {
  name: 'App',

  components: {
    DataComponent
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;

}
</style>
