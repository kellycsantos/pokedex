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

        <v-dialog v-model="drawer"  >
          <div class="details">
            <v-col>
              <v-row class="altura">
                <v-col cols="4">
                  <v-card class="details__card details__card__bio">
                    <v-col>
                      <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${pokemonsDetailsId}.png`">
                    </v-col>
                    <v-divider></v-divider>
                    <v-row>
                      <v-col>#{{pokemonsDetailsId}}</v-col>
                      <v-col>{{pokemonDetailsType}}</v-col>
                    </v-row>
                    

                  </v-card>
                </v-col>
                <v-col>
                  <v-card class="details__card">
                  <v-row>
                    <v-col>Peso</v-col>
                    <v-col>{{pokemonDetailsWeight}}</v-col>
                  </v-row>
                  <v-divider></v-divider>
                  <v-row>
                    <v-col>Altura</v-col>
                    <v-col>{{pokemonDetailsHeight}}</v-col>
                  </v-row>
                  </v-card>
                </v-col>
              </v-row>
            </v-col>
            <v-col>
               <v-card class="details__card">evoluções</v-card>
            </v-col>
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
      drawer: true,
      ativo: '',
      drawer2: true,
      carregando: true,
      pokemonsDetails: [],
      pokemonDetailsImg: '',
      pokemonDetailsType: '',
      pokemonDetailsWeight:'',
      pokemonDetailsHeight:'',
      pokemonsDetailsId: '',


    }
  },
  methods:{
    getImgId(pokemonId: { url: string; }){
      return pokemonId.url.split('/')[6];
    },
    getPokemon(pokemon: string){
      this.ativo = pokemon
      this.drawer = true
      axios
      .get(`https://pokeapi.co/api/v2/pokemon/${this.ativo}`)
      .then((response) => {
        this.pokemonsDetails = response.data
        this.pokemonsDetailsId = response.data.id
        this.pokemonDetailsImg = response.data.sprites.front_default
        this.pokemonDetailsType = response.data.types[0].type.name
        this.pokemonDetailsWeight = response.data.weight
        this.pokemonDetailsHeight = response.data.height
        console.log('Nome:',pokemon)
        console.log('Id:', this.pokemonsDetailsId)
        console.log('Tipo:',this.pokemonDetailsType)
        console.log('Peso:',this.pokemonDetailsWeight)
        console.log('Altura:',this.pokemonDetailsHeight)
        console.log('Imagem:',this.pokemonDetailsImg)
        console.log('Detalhes completo:',this.pokemonsDetails)
      })
      // axios
      // .get(`https://pokeapi.co/api/v2/pokemon-species/${this.ativo}`)
      // .then((response) =>{
      //   console.log(response)
      //   console.log(response.data)
      // })
    }
  },
  created(){
    console.log('carregando')
  },
  mounted(){
    axios
      .get('https://pokeapi.co/api/v2/pokemon?limit=15')
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
  border: 1px solid rgba(189, 182, 182, 0.397);
  padding: 5px;
  display: flex;
  flex-wrap: wrap;
  width: 85vw;
  justify-content: center;
}

.pokemon-group__card{
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 1px solid rgb(0, 26, 255);
  border-radius: 5px;
  height: 290px;
  width: 220px;
  margin: 5px;
  text-transform: capitalize;


}

.pokemon-group__card img{
  width: 75%;
}

.details{
  width: 75vw;
  height: 70vh;
  background: rgb(149, 182, 151);
  border-radius: 10px;
}
.altura{
  height: 50vh;
}

.details__card__bio{
  height: 45vh;
  display: flex;
  flex-direction: column;
  text-align: center;
}

.details__card__bio img{
  width: 100%;
  height: 100%;
}
</style>
