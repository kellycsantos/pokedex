<template>
	<div class="container">
		<img :src="require(`../assets/loading-img/${imgRandom}.jpg`)" />
	</div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
	name: "LoadingPage",
	data() {
		return {
			pokemons: [],
			pokemonImage: this.pokemon,
			drawer: false,
			ativo: "",
			drawer2: true,
			loading: true,
			background: [
				"1",
				"2",
				"3",
				"4",
				"5",
				"6",
				"7",
				"8",
				"9",
				"10",
				"11",
				"12",
			],
			imgRandom: 0,
		};
	},
	methods: {
		getImgId(pokemonId: { url: string }) {
			return pokemonId.url.split("/")[6];
		},
		getPokemon(pokemon: string) {
			this.ativo = pokemon;
			this.drawer = true;
		},
	},
	beforeMount() {
		this.imgRandom = Math.floor(Math.random() * this.background.length);
	}
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
	width: 100vw;
	height: 100vh;
}
img {
	width: 100vw;
	height: 100vh;
}
</style>
