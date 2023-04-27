<template>
    <div class="searchbar">
        <form @submit.prevent="setPokemonUrl">
            <input type="text" v-model="searchvalue">
        </form>
        <i class="fas fa-search" @click="setPokemonUrl"></i>
    </div>
    <div class="typeSelect">
        <ul>
            Type:
            <li v-for="pokemontype in pokemontypes" :key="pokemontype">
                <label>
                    <input type="checkbox" v-model="typeList" :value="pokemontype"
                        :disabled="typeList.length > 1 && typeList.indexOf(pokemontype) === -1" number
                        @change="sortPokemon"> {{ pokemontype }}
                </label>
            </li>
        </ul>
    </div>
</template>
  
<script>
export default {
    props: [
        'apiUrl'
    ],
    data: () => {
        return {
            searchvalue: '',
            typeUrl: 'https://pokeapi.co/api/v2/type/',
            pokemontypes: [],
            typeList: []
        }
    },
    methods: {
        fetchData() {
            let req = new Request(this.typeUrl);
            fetch(req)
                .then((resp) => {
                    if (resp.status === 200)
                        return resp.json();
                })
                .then((data) => {
                    for (let i = 0; i < 18; i++) {
                        this.pokemontypes[i] = data.results[i].name;
                    }
                })
                .catch((error) => {
                    console.log(error);
                })

        },
        setPokemonUrl() {
            if (this.searchvalue !== '')
                this.$emit('setPokemonUrl', this.apiUrl + this.searchvalue);
        },
        sortPokemon() {
            this.$emit('sortPokemon', this.typeList);
        }
    },
    created() {
        this.fetchData();
    }
}
</script>
<style lang="scss" scoped>
.searchbar {
    position: relative;
    width: 100%;
    max-width: 510px;
    padding-bottom: 20px;

    input {
        border: none;
        outline: none;
        border-radius: 5px;
        padding: 10px 40px 10px 10px;
        width: calc(100% - 50px);
        font-size: 1rem;
        box-shadow: 0 15px 30px rgba(0, 0, 0, .2),
            0 10px 10px rgba(0, 0, 0, .2);
    }

    i {
        position: absolute;
        top: 10px;
        right: 10px;
        font-size: 1.25rem;
        color: #0A2E50;
        cursor: pointer;
    }
}

.typeSelect {
    max-width: calc(100% - 1rem);

    ul {
        display: flex;
        flex-wrap: wrap;
        font-size: 1.2rem;

        li {
            list-style: none;

            label {
                font-size: 1.2rem;
                margin-right: 2px;
            }
        }
    }
}</style>