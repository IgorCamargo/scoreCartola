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

    <div class="soughtout" v-bind:class="{ soughtoutContent: isSearch }">
      <soughtout
        v-bind:scoreTotal="dataScoreTotal"
        v-bind:logoTeam="dataLogoTeam"
        v-bind:nameTeam="dataNameTeam"
        v-bind:nameUser="dataNameUser"
        v-bind:logoUser="dataLogoUser"
      ></soughtout>
      <div class="x-close"
        v-if="isSearch">
        <span @click="close">&Cross;</span>
      </div>
    </div>

  </div>
</template>

<script src="https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js"></script>
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
      dataScoreTotal: 0,
      dataSlugTeam: null,
      isSearch: false
    }
  },
  mounted () {
    if(localStorage.getItem('teams')) {
      this.team = localStorage.getItem('teams')
    }
  },
  watch: {
    team: function () {
      localStorage.setItem('teams', this.team)
    }
  },
  methods: {
    searchTeam: function () {

      this.show = true
      var self = this

      if (( this.nameTeam != null ) && ( this.nameTeam != '' )) {
        axios
          .get('https://api.cartolafc.globo.com/times?q='+this.nameTeam)
          .then(function(response){
            if (response.data.error) {
              console.log(response.data.message);
            } else {
              self.isSearch = true

              self.dataSlugTeam = response.data[0].slug
              self.dataNameTeam = response.data[0].nome
              self.dataLogoTeam = response.data[0].url_escudo_svg
              self.dataNameUser = response.data[0].nome_cartola
              self.dataLogoUser = response.data[0].foto_perfil

              if ( self.dataSlugTeam != null ) {
                axios
                  .get('https://api.cartolafc.globo.com/time/slug/'+self.dataSlugTeam)
                  .then(function(response){
                    if (response.data.error) {
                      console.log(response.data.message);
                    } else {
                      // somatório da pontuação total
                      for (var i = 0; i < response.data.rodada_atual; i++) {
                        axios
                          .get('https://api.cartolafc.globo.com/time/slug/'+self.dataSlugTeam+'/'+(i+1))
                          .then(function (response) {
                            self.dataScoreTotal = self.dataScoreTotal + response.data.pontos
                            self.dataScoreTotal = parseFloat(self.dataScoreTotal.toFixed(2))
                          })
                      }
                    }
                  });
                }
            }
          });
      } else {
        console.log('err');
        this.dataScoreTotal = null
        this.dataNameTeam = null
        this.dataLogoTeam = null
        this.dataNameUser = null
        this.dataLogoUser = null
        this.show = false
        this.isSearch = false
      }
    },
    close: function () {
      this.dataScoreTotal = null
      this.dataNameTeam = null
      this.dataLogoTeam = null
      this.dataNameUser = null
      this.dataLogoUser = null
      this.show = false
      this.isSearch = false
    }
  }
}
</script>

<style lang="css">
#scores p {
  text-align: center;
  padding: 4vh;
  border-bottom: 1px solid #000;
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
  height: 0;
  opacity: 0;
  display: flex;
  align-items: center;
  background-color: #e3672a;

  transition: height 1s, opacity 4s;
}
.soughtoutContent {
  height: 25vh !important;
  opacity: 1 !important;

  transition: height 1s, opacity 4s;
}
.x-close {
  position: absolute;
  z-index: 99;
  margin: -8vh 12px;
  padding: 1px 4px;
  right: 0;
  border-radius: 5px;
  color: #883d19;
  font-size: 20px;
  font-weight: bold;
  cursor: pointer;
}
.x-close:hover {
  color: #eea37f;
}
</style>
