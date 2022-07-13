<template>
	<div class="container">	
    <img :src="require(`../assets/loading-img/${imgRandom}.jpg`)">
	</div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from 'axios';
export default defineComponent({
	name: "LoadingPage",
	props: {
		msg: String,
	},
  data(){
    return{
      pokemons: [],
      pokemonImage: this.pokemon,
      drawer: false,
      ativo: '',
      drawer2: true,
      carregando: true,
      background:[
        '1','2','3','4','5','6','7','8','9','10','11','12'
      ],
      imgRandom: 8,
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
  beforeMount(){
  
    this.imgRandom = Math.floor(Math.random() * this.background.length)
      console.log(this.imgRandom)
   
    
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
  .container{ 
    width: 100vw;
    height: 100vh;
  }
  img{
    width: 100vw;
    height: 100vh;

  }
  button{
    background: white;
    height: 70px;
    width: 180px;
    border: 1px solid red;
  }
</style>
