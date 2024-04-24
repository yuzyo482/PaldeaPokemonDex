<template>
  <div class="detail">
    <div v-if="pokemon" class="detail-view">
      <div class="divLeft">
        <div class="image">
          <img :src="imageUrl + pokemon.id + '.png'" alt="">
        </div>
        <div class="data">
          <h2>{{ pokemon.name }}</h2>
          <div class="property">
            <div class="left">Base Experience</div>
            <div class="right">{{ pokemon.base_experience }} XP</div>
          </div>
          <div class="property">
            <div class="left">Height</div>
            <div class="right">{{ pokemon.height / 10 }} m</div>
          </div>
          <div class="property">
            <div class="left">Weight</div>
            <div class="right">{{ pokemon.weight / 10 }} kg</div>
          </div>
          <h3>Pokemon Types</h3>
          <div class="types">
            <div class="type" v-for="(value, index) in pokemon.types" :key="'value' + index">
              {{ value.type.name }}
            </div>
          </div>
        </div>
      </div>
      <div class="divRight">
        <div class="data">
          <h3>Abilities</h3>
          <div class="abilities">
            <div class="ability" v-for="(value, index) in pokemon.abilities" :key="'value' + index">
              {{ value.ability.name }}
            </div>
          </div>
          <h3>Stats</h3>
          <div class="stats">
            <div class="stat" v-for="(value, index) in pokemon.stats" :key="'value' + index">
              <div>{{ value.stat.name }}</div>
              <div class="statBar">{{ value.base_stat }} <div class="statBarColor"
                  :style="{ '--statValue': value.base_stat }"></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <h2 v-else>The pokemon was not found</h2>
    <!-- <i v-else class="fas fa-spinner fa-spin"></i> -->
  </div>
</template>
  
<script>
export default {
  props: [
    'pokemonUrl',
    'imageUrl'
  ],
  data: () => {
    return {
      show: false,
      pokemon: {}
    }
  },
  methods: {
    fetchData() {
      let req = new Request(this.pokemonUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200)
            return resp.json();
        })
        .then((data) => {
          this.pokemon = data;
          this.show = true;
        })
        .catch((error) => {
          console.log(error);
        })
    }

  },
  created() {
    this.fetchData();
  },
  watch: {
    pokemonUrl: function () {
      this.fetchData();
    }
  }
}
</script>
  
<style lang="scss" scoped>
.detail {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 90px 10px 10px;
  width: calc(100% - 20px);

  .detail-view {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: row;
    flex-wrap: wrap;
    position: relative;
    width: 90%;
    max-width: calc(100%-1rem);
    padding: 50px 0 0;
    background-color: #fff;
    border-radius: 5px;
    box-shadow: 0 15px 30px rgba(0, 0, 0, .2),
      0 10px 10px rgba(0, 0, 0, .2);

    .divLeft,
    .divRight {
      width: calc(40% + 1.5rem);
      @media screen and (max-width: 500px) {
        width: calc(80% + 1.5rem);
      }
    }

    .image {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 120px;
      height: 120px;
      background-color: #333;
      border-radius: 50%;
      margin: 0 auto;
      overflow: hidden;
      box-shadow: 0 15px 30px rgba(0, 0, 0, .2),
        0 10px 10px rgba(0, 0, 0, .2);
    }

    h2 {
      text-transform: capitalize;
    }

    .data {
      display: flex;
      justify-content: flex-start;
      align-items: center;
      flex-direction: column;
      width: 100%;
      margin-bottom: 40px;

      .property {
        width: 90%;
        max-width: 400px;
        border-bottom: 1px solid #ccc;
        margin-bottom: 10px;

        .left {
          float: left;
        }

        .right {
          float: right;
        }
      }

      h3 {
        width: 90%;
        max-width: 400px;
        border-bottom: 1px solid #ccc;
      }

      .types,
      .abilities {
        display: flex;
        justify-content: flex-start;
        flex-wrap: wrap;
        width: 90%;
        max-width: 400px;

        .type,
        .ability {
          margin: 0 10px 10px 0;
          padding: 5px 10px;
          border-radius: 20px;
          color: #fff;
          font-size: 1rem;
          letter-spacing: 2px;
          text-transform: capitalize;
          word-wrap: none;
          word-break: keep-all;
        }

        .type {
          background-color: #0A2E50;
        }

        .ability {
          background-color: #C73015;
        }
      }

      .stats {
        width: 60%;

        @media screen and (max-width: 1440px) {
        width: 70%;
      }

        @media screen and (max-width: 1080px) {
        width: 90%;
      }

        @media screen and (max-width: 500px) {
        width: 95%;
      }
        .stat {
          display: flex;
          width: 100%;
          text-transform: capitalize;
          justify-content: space-between;
          

          .statBar {
            position: relative;
            display: block;
            text-align: right;
            width: 60%;
            background-color: #efefef;

            .statBarColor {
              position: absolute;
              top: 2.5%;
              left: 0;
              display: block;
              width: calc(100%*var(--statValue)/255);
              height: 95%;
              background-color: #28c013;
            }
          }
        }
      }
    }

  }

  i {
    font-size: 2rem;
    color: #efefef;
  }
}


</style>