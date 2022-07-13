<template>
	<div class="container">
    <!-- <h1 v-if="carregando">Estou carregando ainda</h1> -->
		<h1  v-if="!carregando">Carreguei</h1> 
      <LoadingPage v-if="carregando"/>

    <div class="pokemon-group"  v-else>
      <v-card class="pokemon-group__card" v-for="pokemon,index in pokemons" :key="index" @click="getPokemon(pokemon.name)">
        <h3>{{pokemon.name}}</h3>
        <h4>#{{getImgId(pokemon)}}</h4>
        <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${getImgId(pokemon)}.png`"
        :alt="pokemon.name">

        <v-dialog v-model="drawer" hide-overlay  >
          <div class="drawer">
            {{ativo}}
          </div>
        </v-dialog>


      </v-card>
    </div>
	</div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from 'axios';

import LoadingPage from '../components/LoadingPage.vue'
export default defineComponent({
	name: "HelloWorld",
	props: {
		msg: String,
	},
  components:{
    LoadingPage
  },
  data(){
    return{
      pokemons: [],
      pokemonImage: this.pokemon,
      drawer: false,
      ativo: '',
      drawer2: true,
      carregando: true,

    }
  },
  methods:{
    getImgId(pokemonId: { url: string; }){
      return pokemonId.url.split('/')[6];
    },
    getPokemon(pokemon: string){
      this.ativo = pokemon
      this.drawer = true
      console.log(pokemon)
    }
  },
  created(){
    console.log('carregando')
  },
  mounted(){
    axios
      .get('https://pokeapi.co/api/v2/pokemon?limit=151')
      .then((response) => {
        this.pokemons = response.data.results
        console.log(response)
        this.carregando = false
      })
  }
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
	margin: 40px 0 0;
}
ul {
	list-style-type: none;
	padding: 0;
}
li {
	display: inline-block;
	margin: 0 10px;
}
a {
	color: #42b983;
}

.container{
  display: flex;
  flex-direction: column;
  align-items: center;
}

.pokemon-group{
  border: 1px solid rgb(158, 143, 143);
  padding: 5px;
  display: flex;
  flex-wrap: wrap;
  width: 85vw;
  justify-content: center;
}

.pokemon-group__card{
  border: 1px solid rgb(0, 26, 255);
  border-radius: 5px;
  height: 290px;
  width: 220px;
  margin: 5px;
  text-transform: capitalize;


}

.pokemon-group__card img{
  width: 80px;
}

.drawer{
  width: 75vw;
  height: 70vh;
  background: rgb(0, 255, 13);
  
}
</style>
