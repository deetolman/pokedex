//this will be results.vue
<template>
    <ul>
        <Pokemon v-for="pokemon in sortedPokemons"
        v-bind:key="pokemon.name"
        v-bind:pokemon="pokemon"/>
    </ul>
</template>

<script>
import pokemonsApi from '../services/pokemonsApi';
import Pokemon from './Pokemon.vue';

export default {
  props: {
    filter: Object,
    sort: Object
  },
  data() {
    return {
      pokemons: pokemonsApi.getPokemons()
    };
  },
  computed: {
    filteredPokemons() {
      const { speed } = this.filter;
      if(!speed) return this.pokemons;
      const { type } = this.type;
      if(!type) return this.pokemons;

      return this.pokemons.filter(pokemon => {
        return pokemon.speed === speed;
      });
    },
    sortedPokemons() {
      let { sort, direction } = this.sort;
      if(!sort) {
        sort = 'name';
      }
            
      return this.filteredPokemons.slice().sort((a, b) => {
        if(a[sort] > b[sort]) return 1 * direction;
        if(b[sort] > a[sort]) return -1 * direction;
        if(b[sort] === a[sort]) return 0;
      });

    }
  },
  components: {
    Pokemon
  }
};
</script>

<style>

</style>
