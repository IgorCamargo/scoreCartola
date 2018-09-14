<template lang="html">
  <div id="scores" class="">

    <!-- exemplo usando localstorage -->
    <!-- <p><input type="text" placeholder="teste" v-model="team"/></p> -->

    <p>
      <input type="text"
        placeholder="search team name"
        v-model="nameTeam"
        autofocus/>
      <button @click="searchTeam">Search</button>
    </p>


    <!-- here, team search -->
    <!-- <div class="result-content"
      v-if="(dataLogoTeam !== null) && (dataNameTeam !== null) && (dataNameUser !== null) && (dataLogoUser !== null)">
      <div class="result-content-logo">
        <img :src="dataLogoTeam" alt="">
      </div>
      <div class="result-content-info">
        <div id="nameTeam">{{ dataNameTeam }}</div>
        <div class="result-content-info-user">
          <img id="logoUser" :src="dataLogoUser" alt="">
          <div id="nameUser">{{ dataNameUser }}</div>
        </div>
      </div>
    </div> -->
    <div class="soughtout">
      <soughtout
      v-bind:logoTeam="dataLogoTeam"
      v-bind:nameTeam="dataNameTeam"
      v-bind:nameUser="dataNameUser"
      v-bind:logoUser="dataLogoUser"
      ></soughtout>
    </div>


  </div>
</template>

<script>
// https://github.com/wgenial/cartrolandofc/blob/master/nova-api.md
import soughtout from './Soughtout.vue'

import axios from 'axios'

export default {
  name: 'scores',
  components: {
    soughtout
  },
  data () {
    return {
      team: 'teste storage',
      nameTeam: null,
      dataNameTeam: null,
      dataLogoTeam: null,
      dataNameUser: null,
      dataLogoUser: null,
      slugTeam: null
    }
  },
  mounted () {
    // axios
    //   .get('https://api.cartolafc.globo.com/rodadas')
    //   .then(response => (this.info = response))
    console.log('mounted');
    if(localStorage.getItem('teams')) {
      this.team = localStorage.getItem('teams')
    }
  },
  watch: {
    team: function () {
      console.log('changed');
      localStorage.setItem('teams', this.team)
    }
  },
  methods: {
    searchTeam: function () {
      var self = this;
      if (( this.nameTeam != null ) && ( this.nameTeam != '' )) {
        axios
        .get('https://api.cartolafc.globo.com/times?q='+this.nameTeam)
        .then(function(response){
          if (response.data.error) {
            console.log(response.data.message);
          } else {
            self.slugTeam = response.data[0].slug
            self.dataNameTeam = response.data[0].nome
            self.dataLogoTeam = response.data[0].url_escudo_svg
            self.dataNameUser = response.data[0].nome_cartola
            self.dataLogoUser = response.data[0].foto_perfil

            // armazeno o slug e se confirmado clicando adiciona com o scrpt abaixo para trazer a soma dos pontos
            // axios
            //   .get('https://api.cartolafc.globo.com/time/slug/'+slug)
            //   .then(function(response){
            //     if (response.data.error) {
            //       console.log(response.data.message);
            //     } else {
            //       var teste = response.data;
            //
            //       console.log(teste.pontos);
            //       console.log(teste.rodada_atual);
            //     }
            //   });
          }
        });
      }
      else {
        console.log('err');
      }
    }
  }
}
</script>

<style lang="css">
#scores p {
  width: 100%;
  text-align: center;
  padding: 4vh;
  border-bottom: 1px solid #000;
  /* background-color: yellow; */
}
#scores p input {
  width: 50%;
  padding: 10px;
  border: 1px solid #D3FFCE;
  text-decoration: none;
  border-radius: 4px;
}
#scores p button {
  padding: 10px;
  margin-left: 10px;
  border: 0;
  border-radius: 4px;
  text-decoration: none;
  background-color: #ddd;
  cursor: pointer;
}
#scores p button:hover {
  background-color: #eee;
}
#scores p button:active {
  background-color: #ddd;
}
#scores .soughtout {
  border-bottom: 1px solid #000;
}
</style>
