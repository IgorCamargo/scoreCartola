<template>

  <div id="app">

    <header>
      <span>Score Cartola</span>
    </header>

    <scores class="scores"></scores>
    <teams class="teams"></teams>

  </div>

</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import scores from './components/Scores.vue'
import teams from './components/Teams.vue'

export default {
  name: 'app',
  components: {
    // HelloWorld
    scores,
    teams
  }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
  vertical-align: baseline;
  list-style: none;
  /* border: 0; */
}
header {
  position: relative;
  width: 100%;
  height: 8vh;
  top: 0;
  display: flex;
  align-items: center;
  font-size: 18px;
  /* background-color: yellow; */
}
header span {
  padding: 0 15px;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#app .scores {
  width: 100%;
  /* height: 10vh; */
  /* display: flex;
  align-items: center; */
  /* background-color: darkred; */
}
#app .teams {
  height: 47vh;
  overflow-y: auto;
  border-top: 1px solid #000;
  background-color: darkblue;
}
</style>
