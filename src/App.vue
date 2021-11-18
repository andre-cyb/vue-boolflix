<template>
  <div id="app">
    <input type="search" v-model="userInput" />
    <button @click="movieCall(`movie`, `movies`), movieCall(`tv`, `series`)">
      Search
    </button>
    <div class="row">
      <ul>
        <li v-for="movie in movies" :key="movie.id">
          <div class="lista_film pb-5">
            <img :src="thumb(movie)" alt="" />
            <h3>
              {{ movie.title }}
            </h3>
            <div>{{ movie.original_title }}</div>
            <div>
              <img :src="lang[movie.original_language]" width="30px" alt="" />
            </div>
            <i
              class="fa fa-star"
              aria-hidden="true"
              v-for="(star, i) in voteFunction(movie)"
              :key="i"
            ></i>
            <i
              class="fa fa-star-o"
              aria-hidden="true"
              v-for="(star, i) in starEmpty(movie)"
              :key="i"
            ></i>
          </div>
        </li>
      </ul>
      <ul>
        <li v-for="serie in series" :key="serie.id">
          <div class="lista_film pb-5">
            <img :src="thumb(serie)" alt="" />
            <h3>
              {{ serie.name }}
            </h3>
            <div>{{ serie.original_name }}</div>
            <div>
              <img :src="lang[serie.original_language]" width="30px" alt="" />
            </div>
            <span
              >Voto:<i
                class="fa fa-star"
                aria-hidden="true"
                v-for="(star, i) in voteFunction(serie)"
                :key="i"
              ></i>
              <i
                class="fa fa-star-o"
                aria-hidden="true"
                v-for="(star, i) in starEmpty(serie)"
                :key="i"
              ></i
            ></span>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  components: {},
  data() {
    return {
      apiKey: "1fef8132a8e5f3adf9eced61ab7c59c8",
      apiUrl: "https://api.themoviedb.org/3",
      imgUrl: "https://image.tmdb.org/t/p/",
      movies: [],
      series: [],

      userInput: "",
      lang: {
        it: require("@/assets/it.svg"),
        en: require("@/assets/gb.svg"),
        fr: require("@/assets/fr.svg"),
        de: require("@/assets/de.svg"),
      },
    };
  },

  methods: {
    movieCall(type, objData) {
      if (!this.userInput) {
        return;
      }
      axios
        .get(this.apiUrl + `/search/` + type, {
          params: {
            api_key: this.apiKey,
            query: this.userInput,
            languages: "en",
          },
        })
        .then((resp) => {
          this[objData] = resp.data.results;
        });
    },
    thumbnailCall() {},
    flag(lang) {
      return this.lang[lang];
    },
    thumb(type) {
      let urlContent = "https://image.tmdb.org/t/p/";
      let idThumb = type.poster_path;
      return urlContent + "w185" + idThumb;
    },
    voteFunction(type) {
      let vote = type.vote_average / 2;
      return Math.ceil(vote);
    },
    starEmpty(type) {
      return 5 - this.voteFunction(type);
    },
  },
  mounted() {},
};
</script>

<style lang="scss">
@import "~bootstrap/scss/bootstrap";
@import "~font-awesome/css/font-awesome.css";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  ul {
    list-style: none;
    width: 50%;
    li {
      list-style: none;
    }
  }
}
</style>
