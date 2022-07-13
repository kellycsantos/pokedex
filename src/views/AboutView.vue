<template>
  <div class="about">
    <LoadingPage/>
  </div>
</template>
<script lang="ts">
import { defineComponent } from "vue";
import LoadingPage from '../components/LoadingPage.vue'
// import axios from 'axios';
export default defineComponent({
	name: "HelloWorld",
	props: {
		msg: String,
	},
  components:{
    LoadingPage,
  }
  // data(){
//     return{
//       pokemons: [],
//       pokemonImage: this.pokemon,
//       drawer: false,
//       ativo: '',
//       drawer2: true,
//       carregando: true,

//     }
//   },
//   methods:{
//     getImgId(pokemonId: { url: string; }){
//       return pokemonId.url.split('/')[6];
//     },
//     getPokemon(pokemon: string){
//       this.ativo = pokemon
//       this.drawer = true
//       console.log(pokemon)
//     }
//   },
//   beforeCreate(){
//     console.log('carregando')
//   },
//   mounted(){
//     axios
//       .get('https://pokeapi.co/api/v2/pokemon?limit=151')
//       .then((response) => {
//         this.pokemons = response.data.results
//         console.log(response)
//         this.carregando = false
//       })
//   }
})
</script>
