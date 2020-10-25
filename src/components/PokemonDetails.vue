<template>
  <div v-if="pokemon == null">Nenhum pokemon selecionado.</div>
  <div v-else>
    <img
      :src="
              'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/' +
              pokemon.id +
              '.png'
            "
      width="325px"
    />
    <h2>{{ pokemon.name }}</h2>
    <p>{{ description }}</p>
    <section style="display: flex; justify-content: center;">
      <div style="width: 250px">
        <h3>Estatísticas</h3>
        <ul style="text-align: left; list-style-type: none">
          <li>
            <b>HP:</b>
            {{ pokemon.stats.hp }}
          </li>
          <li>
            <b>Ataque:</b>
            {{ pokemon.stats.attack }}
          </li>
          <li>
            <b>Defesa:</b>
            {{ pokemon.stats.defense }}
          </li>
          <li>
            <b>Ataque Especial:</b>
            {{ pokemon.stats.specialAttack }}
          </li>
          <li>
            <b>Defesa Especial:</b>
            {{ pokemon.stats.specialDefense }}
          </li>
          <li>
            <b>Velocidade:</b>
            {{ pokemon.stats.speed }}
          </li>
        </ul>
      </div>
      <div style="width: 250px">
        <h3>Tipos</h3>
        <ul style="list-style-type: none;">
          <h4 v-for="type in pokemon.types" :key="type">
            <b-badge pill :class="type">{{ type }}</b-badge>
          </h4>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: {
    pokemon: Object,
  },
  data() {
    return {
      description: "",
    };
  },
  watch: {
    pokemon: "getDescription",
  },
  methods: {
    getDescription() {
      if (this.pokemon) {
        axios
          .get(`https://pokeapi.co/api/v2/pokemon-species/${this.pokemon.id}`)
          .then((result) => {
            let n = Math.trunc(Math.random() * 15);
            let str = result.data.flavor_text_entries[n].flavor_text;
            str = str.replace(/\r?\n|\r|\f/g, " ");
            this.description = str;
          });
        console.log("update");
        // str.replace(/\r?\n|\r|\f/g,' ');
      }
    },
  },
};
</script>

<style scoped>
ul {
  font-size: 1.15rem;
}
.Bug {
  background-color: #a8b820;
  border: 2px solid #6d7815;
}
.Dark {
  background-color: #705848;
  border: 2px solid #49392f;
}
.Dragon {
  background-color: #7038f8;
  border: 2px solid #4924a1;
}
.Electric {
  background-color: #f8d030;
  border: 2px solid #a1871f;
}
.Fairy {
  background-color: #ee99ac;
  border: 2px solid #9b6470;
}
.Fighting {
  background-color: #c03028;
  border: 2px solid #7d1f1a;
}
.Fire {
  background-color: #f08030;
  border: 2px solid #9c531f;
}
.Flying {
  background-color: #a890f0;
  border: 2px solid #6d5e9c;
}
.Ghost {
  background-color: #705898;
  border: 2px solid #493963;
}
.Grass {
  background-color: #78c850;
  border: 2px solid #4e8234;
}
.Ground {
  background-color: #e0c068;
  border: 2px solid #927d44;
}
.Ice {
  background-color: #98d8d8;
  border: 2px solid #638d8d;
}
.Normal {
  background-color: #a8a878;
  border: 2px solid #6d6d4e;
}
.Poison {
  background-color: #a040a0;
  border: 2px solid #682a68;
}
.Psychic {
  background-color: #f85888;
  border: 2px solid #a13959;
}
.Rock {
  background-color: #b8a038;
  border: 2px solid #786824;
}
.Steel {
  background-color: #b8b8d0;
  border: 2px solid #787887;
}
.Water {
  background-color: #6890f0;
  border: 2px solid #445e9c;
}
</style>