<template>
  <!--<div>
    <h1>{{ num }} - {{ upper(name) }}</h1>
    <small>{{ url }}</small>
  </div>-->
  <div class="column is-one-quarter is-half is-offset-one-quarter card" id="pokemon">
    <div class="card-image">
      <figure>
        <img :src="currentImg" alt="Placeholder image">
      </figure>
    </div>
    <div class="card-content">
      <div class="media">
        <div class="media-content">
          <p class="title is-4">{{ num }} - {{ upper(name) }}</p>
          <p class="subtitle is-6">{{ pokemon.type }}</p>
        </div>
      </div>

      <div class="content">
        <div class="buttons">
          <button class="button is-primary" @click="mudarSprite()">Girar</button>
          <button class="button is-link">saber mais</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  created: function() {
    axios.get(this.url).then(res => {
      this.pokemon.type = res.data.types[0].type.name;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.currentImg = this.pokemon.front;

      console.log(res);
    }).catch(e => {
      console.log(e);
    });
  },
  data() {
    return {
      isFront: true,
      currentImg: '',
      pokemon: {
        type: '',
        front: '',
        back: '',
      },
    }
  },
  props: {
    num: Number,
    name: String,
    url: String,
  },
  methods: {
    upper: function(n) {
      var newName = n[0].toUpperCase() + n.slice(1);
      return newName;
    },
    mudarSprite: function() {
      if(this.isFront) {
        this.isFront = false;
        this.currentImg = this.pokemon.back;

      } else {
        this.isFront = true;
        this.currentImg = this.pokemon.front
      }
    }
  },
}
</script>

<style>
#pokemon {
  margin: 1rem;
  width: 300px;
}
</style>