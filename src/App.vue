<template>
  <div id="app">
    <img src="@/assets/pokemon.jpg" alt="Título" class="title-image">
    <h2>¿Quién es ese Pokémon?</h2>
    <p>Número de pokémones descubiertos: {{ pokemonesDescubiertos }}</p>
    <div v-for="(fila, indiceFila) in filasPokemon" :key="indiceFila">
      <PokemonGame 
        :pokemones="fila" 
        :indice-fila="indiceFila" 
        @pokemon-descubierto="manejarPokemonDescubierto" 
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonGame from './components/PokemonGame.vue';

export default {
  components: {
    PokemonGame
  },
  data() {
    return {
      pokemones: []
    };
  },
  computed: {
    pokemonesDescubiertos() {
      return this.pokemones.filter(pokemon => pokemon.descubierto).length;
    },
    filasPokemon() {
      const filas = [];
      for (let i = 0; i < this.pokemones.length; i += 5) {
        filas.push(this.pokemones.slice(i, i + 5));
      }
      return filas;
    }
  },
  methods: {
    async obtenerPokemones() {
      try {
        const respuesta = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=20');
        const datos = respuesta.data.results;
        const promesasDetalles = datos.map(async pokemon => {
          const respuestaDetalle = await axios.get(pokemon.url);
          const datosDetalle = respuestaDetalle.data;
          return {
            nombre: pokemon.name,
            imagen: datosDetalle.sprites.front_default,
            descubierto: false,
            nombreInput: ''
          };
        });
        this.pokemones = await Promise.all(promesasDetalles);
      } catch (error) {
        console.error('Error al obtener datos:', error);
      }
    },
    manejarPokemonDescubierto(pokemon) {
      console.log(`Pokémon descubierto: ${pokemon.nombre}`);
    }
  },
  mounted() {
    this.obtenerPokemones();
  }
};
</script>

<style>
#app {
  text-align: center;
}

.title-image {
  margin: 20px auto;
}
</style>








