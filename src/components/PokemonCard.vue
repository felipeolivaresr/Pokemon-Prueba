<template>
    <div class="pokemon-card" :class="{ 'animate': isDiscovered }">
      <img 
        :src="imageUrl" 
        :style="filterStyle" 
        alt="Pokemon silhouette" 
        class="pokemon-image"
      />
      <div v-if="!isDiscovered">
        <input v-model="userGuess" @keyup.enter="checkGuess"/>
        <button @click="checkGuess">Descubrir</button>
      </div>
      <p v-else>{{ pokemonName }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      pokemonName: String,
      imageUrl: String
    },
    data() {
      return {
        userGuess: '',
        isDiscovered: false,
      };
    },
    computed: {
      filterStyle() {
        return this.isDiscovered ? {} : { filter: 'blur(5px) grayscale(100%)' };
      }
    },
    methods: {
      checkGuess() {
        if (this.userGuess.toLowerCase() === this.pokemonName.toLowerCase()) {
          this.isDiscovered = true;
          this.$emit('pokemonDiscovered');
          this.playAnimation(); 
        } else {
          alert("Nombre incorrecto, intenta de nuevo.");
        }
      },
      playAnimation() {
        
      }
    }
  };
  </script>
  
  <style>
  .pokemon-image {
    width: 150px;
    height: 150px;
  }
  
  .pokemon-card {
    transition: transform 0.3s ease;
  }
  
  .pokemon-card.animate {
    transform: scale(1.1);
    animation: zoom-in 0.5s forwards;
  }
  
  @keyframes zoom-in {
    0% {
      transform: scale(1);
    }
    100% {
      transform: scale(1.2);
    }
  }
  </style>
  