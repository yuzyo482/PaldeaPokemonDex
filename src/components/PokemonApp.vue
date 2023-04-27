<template>
    <div class="container">
      <h1>Paldea Pokemon Dex</h1>
      <PokemonSearch 
        :apiUrl="apiUrl" 
        @setPokemonUrl="setPokemonUrl" 
        @sortPokemon="sortPokemon" />
      <PokemonPaldeaDex
      :imageUrl="imageUrl" 
        :apiUrl="apiUrl"
        :typeList="typeList"
        @setPokemonUrl="setPokemonUrl"/>
      <PokemonDetail 
        :pokemonUrl="pokemonUrl"
        :imageUrl="imageUrl" />
    </div>
  </template>
  
  <script>
    import PokemonSearch from './PokemonSearch.vue';
    import PokemonDetail from './PokemonDetail.vue';
    import PokemonPaldeaDex from './PokemonPaldeaDex.vue';
    export default {
      data: () => {
        return {
          imageUrl: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/',
          apiUrl: 'https://pokeapi.co/api/v2/pokemon/',
          pokemonUrl: 'https://pokeapi.co/api/v2/pokemon/906/',
          typeList:[]
        }
      },
      components: {
        PokemonSearch,
        PokemonDetail,
        PokemonPaldeaDex
      },
      methods: {
        setPokemonUrl(url) {
          this.pokemonUrl = url;
          this.showDetail = true;
        },
        sortPokemon(chosenTypeList){
          this.typeList = chosenTypeList;
        }
      }
    }
  </script>
  
  <style lang="scss" scoped>
    @import url('https://fonts.googleapis.com/css?family=Acme');
    .container {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      padding: 10px;
      width: calc(100% - 20px);
      min-height: calc(100vh - 20px);
      background: radial-gradient(#b9a960, #d1930c);
      font-family: 'Acme', arial;
      font-size: 1rem;
      font-weight: normal;
    }
    h1 { color: #000000; }
  </style>