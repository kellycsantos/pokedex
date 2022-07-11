<template>
	<div class="container">
		<h1>Isso será uma pokedex</h1>
    <div class="pokemon-group">
      <div class="pokemon-group__card" v-for="pokemon,index in pokemons" :key="index">
        <h3>{{pokemon.name}}</h3>
        <h4>{{getImgId(pokemon)}}</h4>
        <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${getImgId(pokemon)}.png`"
        :alt="pokemon.name">
      </div>


    </div>
	</div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from 'axios';
export default defineComponent({
	name: "HelloWorld",
	props: {
		msg: String,
	},
  data(){
    return{
      pokemons: [],
      pokemonImage: this.pokemon
    }
  },
  methods:{
    getImgId(pokemonId: { url: string; }){
      return pokemonId.url.split('/')[6];
    }
  },
  mounted(){
    axios
      .get('https://pokeapi.co/api/v2/pokemon?limit=151')
      .then((response) => {
        this.pokemons = response.data.results
        console.log(response)
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


}

.pokemon-group__card img{
  width: 80px;
}
</style>
