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
    border-radius: 10px;
    border: 2px solid #ffcc00;
  }
  
  .pokemon-card {
    background-color: #ffffff;
    border: 2px solid #ff0000;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    padding: 10px;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }
  
  .pokemon-card:hover {
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
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
  