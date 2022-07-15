<template>
	<div class="container">
		<LoadingPage v-if="loading" />

		<div class="pokemon-group" v-else>
			<v-card
				class="pokemon-group__card"
				v-for="(pokemon, index) in pokemons"
				:key="index"
				@click="getPokemon(index + 1)"
			>
				<h3>{{ pokemon.name }}</h3>
				<h4>#{{ getImgId(pokemon) }}</h4>
				<span class="card__img-container">
					<img
						:src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${getImgId(
							pokemon
						)}.png`"
						:alt="pokemon.name"
					/>
				</span>
			</v-card>
		</div>

		<v-dialog v-model="dialogActive">
			<div
				class="details"
				:class="{
					'details--fire': pokemonDetailsType == 'fire',
					'details--grass': pokemonDetailsType == 'grass',
					'details--water': pokemonDetailsType == 'water',
					'details--bug': pokemonDetailsType == 'bug',
					'details--normal': pokemonDetailsType == 'normal',
					'details--ground': pokemonDetailsType == 'ground',
					'details--fighting': pokemonDetailsType == 'fighting',
					'details--poison': pokemonDetailsType == 'poison',
					'details--psychic': pokemonDetailsType == 'psychic',
					'details--ice': pokemonDetailsType == 'ice',
					'details--fairy': pokemonDetailsType == 'fairy',
					'details--electric': pokemonDetailsType == 'electric',
					'details--dragon':pokemonDetailsType == 'dragon',
					'details--ghost' :pokemonDetailsType == 'ghost',
					'details--rock' :pokemonDetailsType == 'rock'
				}"
			>
				<v-col>
					<v-row class="altura">
						<v-col cols="4">
							<v-card class="details__card details__card__bio">
								<v-col>
									<img
										:src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${pokemonsDetailsId}.png`"
									/>
								</v-col>

								<v-col>
									<h2 class="details__card__title">
										{{ pokemonsDetailsName }}
									</h2>
								</v-col>

								<v-divider></v-divider>
								<v-row>
									<v-col>
										<span class="details__card__square"
											>#{{ pokemonsDetailsId }}</span
										>
									</v-col>
									<v-col>
										<span
											class="details__card__square"
											:class="{
												'details__card__square--fire':
													pokemonDetailsType ==
													'fire',
												'details__card__square--grass':
													pokemonDetailsType ==
													'grass',
												'details__card__square--water':
													pokemonDetailsType ==
													'water',
												'details__card__square--bug':
													pokemonDetailsType == 'bug',
												'details__card__square--normal':
													pokemonDetailsType ==
													'normal',
												'details__card__square--ground':
													pokemonDetailsType ==
													'ground',
												'details__card__square--fighting':
													pokemonDetailsType ==
													'fighting',
												'details__card__square--poison':
													pokemonDetailsType ==
													'poison',
												'details__card__square--psychic':
													pokemonDetailsType ==
													'psychic',
												'details__card__square--ice':
													pokemonDetailsType == 'ice',
												'details__card__square--fairy':
													pokemonDetailsType ==
													'fairy',
												'details__card__square--electric':
													pokemonDetailsType ==
													'electric',
												'details__card__square--dragon':
													pokemonDetailsType ==
													'dragon',	
												'details__card__square--ghost':
													pokemonDetailsType ==
													'ghost',
												'details__card__square--rock':
													pokemonDetailsType ==
													'rock',			
											}"
											>{{ pokemonDetailsType }}</span
										>
									</v-col>
								</v-row>
							</v-card>
						</v-col>
						<v-col>
							<v-card class="details__card details__card__info">
								<v-row>
									<v-col
										><p class="details__card__title">
											HP
										</p></v-col
									>
									<v-col
										>{{ pokemonsDetailsHp }}</v-col
									>
								</v-row>
								<v-divider></v-divider>								
								<v-row>
									<v-col
										><p class="details__card__title">
											weight
										</p></v-col
									>
									<v-col
										>{{ pokemonDetailsWeight }} Kgs</v-col
									>
								</v-row>
								<v-divider></v-divider>
								<v-row>
									<v-col
										><p class="details__card__title">
											height
										</p></v-col
									>
									<v-col
										>{{ pokemonDetailsHeight }}0 cm</v-col
									>
								</v-row>
								<v-divider></v-divider>
								<v-row>
									<v-col
										><p class="details__card__title">
											Type
										</p></v-col
									>
									<v-col
										>{{ pokemonDetailsType }}</v-col
									>
								</v-row>
								<v-divider></v-divider>
							</v-card>
						</v-col>
					</v-row>
				</v-col>
			</div>
		</v-dialog>
	</div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import axios from "axios";

import LoadingPage from "./LoadingPage.vue";
export default defineComponent({
	name: "PokeDex",
	props: {
		msg: String,
	},
	components: {
		LoadingPage,
	},
	data() {
		return {
			pokemons: [],
			pokemonsColors: [],
			ativo: "",
			dialogActive: false,
			loading: true,
			pokemonsDetails: [],
			pokemonDetailsImg: "",
			pokemonDetailsType: "",
			pokemonDetailsWeight: "",
			pokemonDetailsHeight: "",
			pokemonsDetailsId: "",
			pokemonsDetailsName: "",
			pokemonsDetailsHp: "",
		};
	},
	methods: {
		getImgId(pokemonId: { url: string }) {
			return pokemonId.url.split("/")[6];
		},
		getPokemon(pokemon: string) {
			this.ativo = pokemon;
			axios
				.get(`https://pokeapi.co/api/v2/pokemon/${this.ativo}`)
				.then((response) => {
					this.pokemonsDetails = response.data;
					this.pokemonsDetailsId = response.data.id;
					this.pokemonsDetailsHp = response.data.stats[0].base_stat;
					this.pokemonsDetailsName = response.data.name;
					this.pokemonDetailsImg =
						response.data.sprites.front_default;
					this.pokemonDetailsType = response.data.types[0].type.name;
					this.pokemonDetailsWeight = response.data.weight;
					this.pokemonDetailsHeight = response.data.height;
				});
			this.dialogActive = true;
			
		},
	},
	mounted() {
		axios
			.get("https://pokeapi.co/api/v2/pokemon?limit=151")
			.then((response) => {
				this.pokemons = response.data.results;
				this.loading = false;
			});
	},
});
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

.container {
	display: flex;
	flex-direction: column;
	align-items: center;
	background: url("../assets/background/pokemon-in-the-wild.png") no-repeat
		center fixed;
	background-size: cover;
}

.pokemon-group {
	padding: 30px 5px 10px 5px;
	display: flex;
	flex-wrap: wrap;
	width: 90vw;
	justify-content: center;
}

.pokemon-group__card {
	display: flex;
	flex-direction: column;
	align-items: center;
	border-radius: 5px;
	height: 290px;
	width: 220px;
	margin: 10px;
	text-transform: capitalize;
	background: #d9ece4;
}

.card__img-container {
	text-align: center;
	border-radius: 50%;
	margin: 15px;
	background: #fcfaf873;
}

.pokemon-group__card img {
	width: 85%;
	--webkit-filter: drop-shadow(5px 5px 5px #222);
	filter: drop-shadow(5px 5px 5px rgb(156, 156, 156));
}

.details {
	width: 75vw;
	max-width: 880px;
	height: 54vh;
	background: #d9e4da;
	border-radius: 10px;
}


.details--poition {
	width: 75vw;
	height: 70vh;
	background: rgb(149, 182, 151);
	background: linear-gradient(54deg, #ffd92a 11%, #df7810 100%);
	border-radius: 10px;
}

.details__card__bio {
	height: 48vh;
	display: flex;
	flex-direction: column;
	text-align: center;
	background: #fffefe38;
}
.details__card__title {
	text-transform: capitalize;
	font-weight: 700;
}
.details__card__bio img {
	width: 100%;
	height: 100%;
	max-width: 400px;
	--webkit-filter: drop-shadow(5px 5px 5px#474747cc);
	filter: drop-shadow(5px 5px 5px #474747cc);
}

.details__card__info {
	height: 48vh;
	font-size: 30px;
	padding: 1vh 2vw;
	background-blend-mode: darken;
	background: #fffefe38;
}

.details__card__square {
	display: inline-flex;
	justify-content: center;
	align-items: center;
	width: 8vw;
	max-width: 110px;
	height: 30px;
	margin-top: 15px;
	color: #fff;
	background: #00000042;
}

.details__card__square--ice {
	background: #68edf1;
}
.details__card__square--bug {
	background: #83e207;
}
.details__card__square--fire {
	background: #ff8800;
}
.details__card__square--grass {
	background: #24ad18;
}
.details__card__square--water {
	background: #34b1e2;
}
.details__card__square--fairy {
	background: #e49bda;
}
.details__card__square--normal {
	background: #dfd8c5;
}
.details__card__square--ground {
	background: #bb7e4d;
}
.details__card__square--poison {
	background: #8534e2;
}
.details__card__square--psychic {
	background: #d82eb3;
}
.details__card__square--fighting {
	background: #e23434;
}
.details__card__square--electric {
	background: #d4c706;
}
.details__card__square--dragon{
	background:#dca5fc;
}
.details__card__square--ghost{
	background:#450d66
}
.details__card__square--rock{
	background:#5f5c5c
}
/* teste */
.details--ice {
	background: linear-gradient(
		45deg,
		rgba(104, 237, 241, 1) 0%,
		rgba(20, 145, 149, 1) 100%
	);
}
.details--bug {
	background: linear-gradient(120deg, #d4fc79 0%, #96e6a1 100%);
}
.details--fire {
	background: radial-gradient(
		circle at 10% 20%,
		rgb(255, 197, 61) 0%,
		rgb(255, 94, 7) 90%
	);
}
.details--grass {
	background: radial-gradient(
		circle at 10% 20%,
		rgb(4, 159, 108) 0%,
		rgb(194, 254, 113) 90.1%
	);
}
.details--water {
	background: radial-gradient(
		circle at 0.7% 1%,
		rgb(215, 248, 247) 0%,
		rgb(102, 188, 239) 100.2%
	);
}
.details--fairy {
	background: radial-gradient(
		circle at 5.3% 17.2%,
		rgb(255, 208, 253) 0%,
		rgb(255, 237, 216) 90%
	);
}
.details--normal {
	background: linear-gradient(135deg, #fdfcfb 0%, #e2d1c3 100%);
}
.details--ground {
	background: linear-gradient(
		87.4deg,
		rgb(255, 241, 165) 1.9%,
		rgb(200, 125, 76) 49.7%,
		rgb(83, 54, 54) 100.5%
	);
}
.details--poison {
	background: linear-gradient(
		109.6deg,
		rgb(218, 185, 252) 11.2%,
		rgb(125, 89, 252) 91.1%
	);
}
.details--psychic {
	background: linear-gradient(
		to right,
		rgb(250, 112, 154) 0%,
		rgb(254, 225, 64) 100%
	);
}
.details--fighting {
	background: radial-gradient(
		circle at 10% 20%,
		rgb(255, 197, 118) 0%,
		rgb(254, 106, 103) 47.7%,
		rgb(240, 23, 23) 92.3%
	);
}
.details--electric {
	background: radial-gradient(
		circle at 10% 20%,
		rgb(254, 255, 165) 0%,
		rgb(255, 232, 182) 90%
	);
}
.details--dragon {
	background: linear-gradient(
		68.6deg,
		rgb(252, 165, 241) 1.8%,
		rgb(181, 255, 255) 100.5%
	);
}

.details--rock {
	background: linear-gradient(to top, #c4c5c7 0%, #dcdddf 52%, #ebebeb 100%);
}

.details--ghost{
	
background: linear-gradient(to right, rgb(173, 83, 137), rgb(60, 16, 83));

}

.details--steel {
	background: linear-gradient(
			to bottom,
			rgba(255, 255, 255, 0.15) 0%,
			rgba(0, 0, 0, 0.15) 100%
		),
		radial-gradient(
				at top center,
				rgba(255, 255, 255, 0.4) 0%,
				rgba(0, 0, 0, 0.4) 120%
			)
			#989898;
}
</style>
