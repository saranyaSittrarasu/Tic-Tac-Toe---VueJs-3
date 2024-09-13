<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <div>
    <h1 class="text-center text-3xl">Tic Tac Toe</h1>
    <hr />
  <TicTacToe />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import TicTacToe from './components/TicTacToe.vue'
import './assets/tailwind.css'
export default {
  name: 'App',
  components: {
    // HelloWorld,
    TicTacToe
  }
}
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
