<template>
  <div id="App">

    <img src="./assets/Pokemon-Logo.png" class="logo-pok">
    <h4 class="is-size-4">Pokedex</h4>

    <div class="field has-addons column is-4 is-offset-8">
      <div class="control">
        <input type="text" name="" id="" placeholder="Buscar Pokemon ..." v-model="busca" class="input">
      </div>
      <div class="control">
        <a class="button is-info" @click="buscar()">Buscar</a>
      </div>
    </div>

    <div id="area-pokemon" class="container columns is-gapless is-multiline is-mobile">
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index+1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pokemon from "./components/Pokemon";

export default {
  name: 'App',
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: '',
    }
  },
  created: function() {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
      //console.log("Pegou a lista");

    }).catch(e => {
      console.log(e);
    })
  },
  methods: {
    buscar: function() {
      this.filteredPokemons = this.pokemons;
      if(this.busca == '' || this.busca == ' ') {              
        this.filteredPokemons = this.pokemons;
      } else {         
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.toLowerCase().includes(this.busca.toLowerCase()))  
      }    
    }
  },
  components: {
    Pokemon,
  },
  computed: {     
    /*resultadoBusca: function() {       
      if(this.busca == '' || this.busca == ' ') {         
        return this.pokemons;

      } else {         
        return this.pokemons.filter(pokemon => pokemon.name.toLowerCase().includes(this.busca.toLowerCase()) )    
      }    
    } */  
  },
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
.logo-pok {
  max-width: 400px;
}
figure {
  background: #F2F2F2;
}
#area-pokemon {
  margin: 0 auto;
}
</style>
