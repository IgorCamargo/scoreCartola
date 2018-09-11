<template lang="html">

  <div id="soughtout" class="result-content"
    v-if="(logoTeam !== null) && (nameTeam !== null) && (nameUser !== null) && (logoUser !== null)">
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
  </div>

</template>

<script>
export default {
  name: 'soughtout',
  props: ['logoTeam','nameTeam','nameUser','logoUser'],
  data () {
    return {
      teste: null
    }
  }
}
</script>

<style lang="css">
</style>
