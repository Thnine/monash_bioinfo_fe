<template>
  <div id="app">
    <box1/>
  </div>
</template>

<script>
import box1 from "@/components/box1.vue"


export default {
  name: 'App',
  components: {
    box1
  }
}
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: #f8f9fa;
  display: flex;
  justify-content: center;
  align-items: center;
  width: calc(100vw - 18px);
  height: calc(100vh - 18px);

}
</style>
