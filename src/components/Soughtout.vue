<template lang="html">

  <div id="soughtout" class="result-content"
    @click="teste"
    v-if="(scoreTotal !== null) && (logoTeam !== null) && (nameTeam !== null) && (nameUser !== null) && (logoUser !== null)">
    <div class="result-content-logo">
      <img :src="logoTeam" alt="">
    </div>
    <div class="result-content-info">
      <div id="nameTeam">{{ nameTeam }}</div>
      <div class="result-content-info-user">
        <img id="logoUser" :src="logoUser" alt="">
        <div id="nameUser">{{ nameUser }}</div>
      </div>
    </div>
    <div class="result-content-score">
      {{ scoreTotal }}
    </div>
  </div>

</template>

<script>
export default {
  name: 'soughtout',
  props: ['scoreTotal','logoTeam','nameTeam','nameUser','logoUser'],
  methods: {
    teste: function () {
      alert('hey brother')
    }
  }
}
</script>

<style lang="css">
.result-content {
  width: 80vw;
  padding: 1em;
  margin: 0 auto;
  display: flex;
  align-items: center;
  cursor: pointer;
  color: #000;
  background-color: #f0f3f5;
  border: 1px solid #bcb8ff;
  border-radius: 5px;
}
.result-content:hover {
  background-color: #d8dadc;
}
.result-content-logo {
  width: 25%;
}
.result-content-logo img {
  width: 75px;
  height: 75px;
}
.result-content-info {
  width: 60%;
}
.result-content-score {
  width: 15%;
  color: #989bab;
  font-weight: bold;
}
#nameTeam {
  text-align: left;
  font-size: 22px;
}
.result-content-info-user {
  display: flex;
  align-items: center;
  padding: 0.5em 0;
}
#logoUser {
  width: 30px;
  height: 30px;
  border: 1px solid #000;
  border-radius: 5px;
}
#nameUser {
  padding: 0 1em;
  font-size: 12px;
  color: #989bab;
}
</style>
