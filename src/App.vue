<template>
  <div id="app">
    <ProgressIndicator v-show="loading" />
    <div class="wrapper">
      <div class="one">
        <PokemonDetails :pokemon="selectedPokemon" />
      </div>
      <div class="two">
        <Catalog :pokeList="pokemon" @row-selected="pokemonSelected" />
      </div>
    </div>
  </div>
</template>

<script>
import Catalog from "./components/Catalog.vue";
import PokemonDetails from "./components/PokemonDetails.vue";
import ProgressIndicator from "./components/ProgressIndicator.vue";
import axios from "axios";

class Pokemon {
  constructor(id, name, stats, types) {
    this.id = id;
    this.name = name;
    this.stats = stats;
    this.types = types;
  }

  get typesToString() {
    let strTypes = this.types[0];
    for (let i = 1; i < this.types.length; i++) {
      strTypes += "/" + this.types[i];
    }
    return strTypes;
  }
}

class Stats {
  constructor(hp, attack, defense, specialAttack, specialDefense, speed) {
    this.hp = hp;
    this.attack = attack;
    this.defense = defense;
    this.specialAttack = specialAttack;
    this.specialDefense = specialDefense;
    this.speed = speed;
  }
}

export default {
  name: "App",
  components: {
    Catalog,
    PokemonDetails,
    ProgressIndicator,
  },
  created: function () {
    this.getData();
  },
  data() {
    return {
      counter: 1,
      loading: true,
      pokemon: [],
      selectedPokemon: null,
    };
  },
  methods: {
    pokemonSelected(pokemon) {
      this.selectedPokemon = pokemon;
    },
    getData() {
      if (this.counter <= 151) {
        axios
          .get(`https://pokeapi.co/api/v2/pokemon/${this.counter}`)
          .then((result) => {
            let types = [];
            for (let t of result.data.types) {
              types.push(
                t.type.name.charAt(0).toUpperCase() + t.type.name.slice(1)
              );
            }
            let poke = new Pokemon(
              result.data.id,
              result.data.name.charAt(0).toUpperCase() +
                result.data.name.slice(1),
              new Stats(
                result.data.stats[0].base_stat,
                result.data.stats[1].base_stat,
                result.data.stats[2].base_stat,
                result.data.stats[3].base_stat,
                result.data.stats[4].base_stat,
                result.data.stats[5].base_stat
              ),
              types
            );
            this.pokemon.push(poke);
            this.counter++;
            this.getData();
          });
      } else {
        this.loading = false;
      }
    },
  },
};
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  overflow: hidden;
}
.wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 10px;
}
.one {
  grid-column: span 2;
  grid-row: 1;
}
.two {
  grid-column: 3;
  grid-row: 1;
  height: 100vh;
}
</style>
