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
import scores from './components/Scores.vue'
import teams from './components/Teams.vue'

export default {
  name: 'app',
  components: {
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
}
body {
  height: 100vh;
  background-color: #f2f2f2;
}
header {
  position: fixed;
  width: 100%;
  height: 8vh;
  top: 0;
  display: flex;
  align-items: center;
  font-size: 18px;
  font-weight: bold;
  color: #fff;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  border-bottom: 2px solid #9e481d;
  background-color: #e3672a;
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
  margin-top: 8vh;
}
#app .teams {
  overflow-y: auto;
  border-top: 1px solid #000;
}
</style>
