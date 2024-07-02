<template>
  <div>
    <div class="fila-pokemon">
      <div v-for="(pokemon, indice) in pokemones" :key="indice" class="carta-pokemon">
        <img :src="pokemon.imagen" :class="{ filtered: !pokemon.descubierto }" alt="Silueta de Pokémon">
        <p v-show="pokemon.descubierto">{{ pokemon.nombre }}</p>
        <div v-show="!pokemon.descubierto">
          <input type="text" v-model="pokemon.nombreInput" @keyup.enter="verificarRespuesta(pokemon)">
          <button @click="verificarRespuesta(pokemon)">Descubrir</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    pokemones: {
      type: Array,
      required: true
    },
    indiceFila: {
      type: Number,
      required: true
    }
  },
  computed: {
    filasPokemon() {
      const filas = [];
      for (let i = 0; i < this.pokemones.length; i += 5) {
        filas.push(this.pokemones.slice(i, i + 5));
      }
      return filas;
    }
  },
  methods: {
    verificarRespuesta(pokemon) {
      if (pokemon.nombreInput.toLowerCase() === pokemon.nombre) {
        pokemon.descubierto = true;
        this.$emit('pokemon-descubierto', pokemon);
      } else {
        alert('Nombre incorrecto');
      }
      pokemon.nombreInput = '';
    }
  }
};
</script>

<style scoped>
.fila-pokemon {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.carta-pokemon {
  flex: 1;
  margin: 0 5px;
  text-align: center;
}

.filtered {
  filter: blur(5px) grayscale(100%);
}

.contador-descubiertos {
  margin-top: 20px;
}

/* Estilos adicionales para el input */
.carta-pokemon input {
  margin-bottom: 5px;
}
</style>





  
  
  
  
  
  
  