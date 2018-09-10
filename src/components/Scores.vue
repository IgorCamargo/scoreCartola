<template lang="html">
  <div class="">

    <!-- exemplo usando localstorage -->
    <p><input type="text" placeholder="teste" v-model="team"/></p>

    <p>
      <input type="text" placeholder="search team name" v-model="nameTeam"/>
      <button @click="searchTeam">Search</button>
    </p>

    <!-- here, list teams searcheds -->
    <div class="result-content"
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
    </div>

  </div>
</template>

<script>
// https://github.com/wgenial/cartrolandofc/blob/master/nova-api.md
import axios from 'axios'

export default {
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
  }
}
</script>

<style lang="css">
.result-content {
  width: 80vw;
  padding: 2em;
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
  width: 75%;
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
