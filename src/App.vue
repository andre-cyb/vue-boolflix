<template>
  <div id="app">
    <input type="search" v-model="userInput" />
    <button @click="movieCall(`movie`, `movies`), movieCall(`tv`, `series`)">
      Search
    </button>
    <ul>
      <li v-for="(movie, i) in movies" :key="i">
        <ul class="lista_film pb-5">
          <li>
            <strong>{{ movie.title }}</strong>
          </li>
          <li>{{ movie.original_title }}</li>
          <li>{{ lang[movie.original_language] }}</li>
          <li>{{ movie.vote_average }}</li>
        </ul>
      </li>
    </ul>
    <ul>
      <li v-for="(serie, i) in series" :key="i">
        <ul class="lista_film pb-5">
          <li>
            <strong>{{ serie.name }}</strong>
          </li>
          <li>{{ serie.original_name }}</li>
          <li>{{ lang[serie.original_language] }}</li>
          <li>{{ serie.vote_average }}</li>
        </ul>
      </li>
    </ul>
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
        it: "bandiera italiana",
        en: "bandiera inglese",
        fr: "bandiera francese",
        de: "bandiera tedesca",
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
  },
  mounted() {},
};
</script>

<style lang="scss">
@import "~bootstrap/scss/bootstrap";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
