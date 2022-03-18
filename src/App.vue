<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-12">
          <h1>{{ titulo }}</h1>
        </div>
        <div class="col-12 d-flex">
          <h2>Nombre</h2>
          <input type="text" class="form-control" v-model="pokemonDefecto" />
          <button class="btn btn-primary" @click="buscarPokemon">Buscar</button>
        </div>

        <div class="col-12">
          <img :src="pokeImagen" :alt="pokeName" />
          <p>{{ pokeName }}</p>
        </div>

        <div class="col-12">
          <h3>Movimientos</h3>
          <ul>
            <li v-for="(pokeMovimiento, i) in pokeMovimientos" :key="i">
              {{ pokeMovimiento }}
            </li>
          </ul>

          <p>Transform</p>
        </div>
        <div class="col-12">
          <h3>Habilidades</h3>
          <div v-for="(pokehablidad, i) in pokehablidades" :key="i">
            {{ pokehablidad }}
          </div>
          <p>Imposter limber</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      URL_PRINCIPAL: "https://pokeapi.co/api/v2/pokemon/",
      pokemonDefecto: "pikachu",
      pokemonNew: {},
    };
  },

  computed: {
    titulo() {
      const titulo = "PokeGuia";
      return titulo.toLocaleUpperCase();
    },
    pokeImagen() {
      return this.pokemonNew?.sprites?.other?.dream_world?.front_default;
    },

    pokeMovimientos() {
      return this.pokemonNew?.moves?.map((a) => a.move.name);
    },

    pokehablidades() {
      return this.pokemonNew?.abilities?.map((a) => a.ability.name);
    },

    pokeName() {
      return this.pokemonNew?.name;
    },
  },
  created() {
    this.obtenerPokemon();
  },
  methods: {
    async obtenerPokemon() {
      try {
        const reques = await fetch(
          `${this.URL_PRINCIPAL}${this.pokemonDefecto}`
        );
        const datos = await reques.json();
        this.pokemonNew = datos;
        console.log(datos);
      } catch (error) {
        console.error(error);
        (this.pokemonDefecto = "pikachu"), this.obtenerPokemon();
      }
    },
    buscarPokemon() {
      this.obtenerPokemon();
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
ul {
  list-style: none;
}
</style>
