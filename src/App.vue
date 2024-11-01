<template>
  <div id="app">
    <header class="pokemon-header">
      <img src="https://1000marcas.net/wp-content/uploads/2020/01/Pok%C3%A9mon-emblema.jpg" alt="Pokémon Logo" class="pokemon-logo">
    </header>
    <h1 class="pokemon-title">¿Quién es ese Pokémon?</h1>
    <p class="pokemon-subtitle">Pokémones descubiertos: {{ discoveredCount }}/20</p>
    <div class="pokemon-grid">
      <PokemonCard 
        v-for="pokemon in pokemons" 
        :key="pokemon.name"
        :pokemonName="pokemon.name"
        :imageUrl="pokemon.imageUrl"
        @pokemonDiscovered="increaseCount"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';

export default {
  components: {
    PokemonCard
  },
  data() {
    return {
      pokemons: [],
      discoveredCount: 0
    };
  },
  methods: {
    async fetchPokemons() {
      try {
        const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=20');
        const pokemonData = await Promise.all(
          response.data.results.map(async (pokemon) => {
            const pokemonDetail = await axios.get(pokemon.url);
            return {
              name: pokemon.name,
              imageUrl: pokemonDetail.data.sprites.front_default
            };
          })
        );
        this.pokemons = pokemonData;
      } catch (error) {
        console.error("Error al obtener datos de PokéAPI:", error);
      }
    },
    increaseCount() {
      this.discoveredCount++;
    }
  },
  mounted() {
    this.fetchPokemons();
  }
};
</script>

<style>

.pokemon-header {
  text-align: center;
  margin-top: 20px;
  margin-bottom: 20px;
}

.pokemon-logo {
  width: 100%; 
  height: 100%;
}


.pokemon-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr); 
  grid-template-rows: repeat(5, auto);   
  gap: 20px;
  justify-items: center; 
  margin: 0 auto;
  max-width: 1000px; 
}


.pokemon-title {
  text-align: center;
  margin-top: 20px;
  margin-bottom: 20px;
  color: white;
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
}


#app{
  background-color: black;
}

.pokemon-subtitle {
  text-align: center;
  margin-top: 20px;
  margin-bottom: 20px;
  color: yellow;
  font-weight: bold;
}
</style>
