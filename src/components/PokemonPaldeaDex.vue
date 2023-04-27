<template>
  <div class="dexBox" @wheel="scrollHorizontal($event)">
    <div class="dexUnion" v-for="(pokemon, index) in  pokemons " :key="'poke' + index" :style="pokemon"
      @click="setPokemonUrl(pokemon.pokemon_species.url)">
      <img :src="imageUrl + pokemon.id + '.png'" width="48" height="48" alt="">
      <h3>{{ pokemon.pokemon_species.name }}</h3>
    </div>
  </div>
</template>

<script>

export default {
  props: {
    typeList: {
      type: Array,
      required: true
    }
  },
  data: () => {
    return {
      imageUrl: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/',
      apiUrl: 'https://pokeapi.co/api/v2/pokemon/',
      pokemons: [],
      currentUrl: '',
      paldeaUrl: 'https://pokeapi.co/api/v2/pokedex/31/'
    }
  },
  methods: {
    fetchData() {
      let req = new Request(this.currentUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200)
            return resp.json();
        })
        .then((data) => {
          data.pokemon_entries.forEach(pokemon => {
            // get the pokemon's id
            pokemon.id = pokemon.pokemon_species.url.split('/')
              .filter(function (part) { return !!part }).pop();
            // get the pokemon's type from other apiUrl
            let pokemonUrl = this.apiUrl + pokemon.id;
            let req2 = new Request(pokemonUrl);
            fetch(req2)
              .then((resp) => {
                if (resp.status === 200)
                  return resp.json();
              })
              .then((data) => {
                pokemon.types = data.types;
                pokemon.display = "inline-block";
              })
              .catch((error) => {
                console.log(error);
              });
            this.pokemons.push(pokemon);
          });
        })
        .catch((error) => {
          console.log(error);
        })
    },
    setPokemonUrl(url) {
      let trueUrl = url.replace('pokemon-species', 'pokemon');
      this.$emit('setPokemonUrl', trueUrl);
    },
    scrollHorizontal(event) {
      event.preventDefault();
      event.currentTarget.scrollLeft += event.deltaY*2.5;
    }
  },
  watch: {
    typeList: function (newVal) { // watch it
      if (newVal.length != 0) {
        this.pokemons.forEach(function (pokemon) {
          pokemon.display = 'inline-block';
          if (newVal.length == 1) {
            if (pokemon.types.length == 1) {
              (pokemon.types[0].type.name != newVal[0]) && (pokemon.display = 'none');
            } else {
              (pokemon.types[0].type.name != newVal[0] && pokemon.types[1].type.name != newVal[0]) && (pokemon.display = 'none');
            }
          } else {
            if (pokemon.types.length == 1) {
              (pokemon.types[0].type.name != newVal[0] && pokemon.types[0].type.name != newVal[1]) && (pokemon.display = 'none');
            } else {
              (pokemon.types[0].type.name != newVal[0] && pokemon.types[1].type.name != newVal[0] && pokemon.types[0].type.name != newVal[1] && pokemon.types[1].type.name != newVal[1]) && (pokemon.display = 'none');
            }
          }
        })
      } else {
        this.pokemons.forEach(function (pokemon) {
          pokemon.display = 'inline-block';
        })
      }
    }
  },
  created() {
    this.currentUrl = this.paldeaUrl;
    this.fetchData();
  }

}

</script>

<style lang="scss" scoped>
.dexBox {
  display: flex;
  height: 180px;
  width: 100%;
  max-width: calc(100% - 2rem);
  overflow-x: auto;
  overflow-y: hidden;
  scrollbar-width: none;
  scroll-behavior: smooth;

  .dexUnion {
    display: inline-block;
    height: 160px;
    width: 50px;
    border: 1px solid #000000;
    background-color: #efefef;
    text-align: center;
    text-transform: capitalize;
    word-wrap: none;
    border-radius: 5px;
    cursor: pointer;
    box-shadow: 0 15px 30px rgba(0, 0, 0, .2),
      0 10px 10px rgba(0, 0, 0, .2);

    h3 {
      margin: 0;
      margin-top: 4px;
      transform: rotate(90deg);
    }
  }
}
</style>