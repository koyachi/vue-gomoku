<template>
  <div id="app">
    <!--
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    //-->
    <Board :size="10"></Board>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import Board from "./components/Board.vue";

export default {
  name: "app",
  components: {
    HelloWorld,
    Board
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
