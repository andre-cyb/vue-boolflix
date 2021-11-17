<template>
  <div id="app">
    <input type="search" v-model="userInput" />
    <button @click="movieCall">Search</button>
    <ul>
      <li v-for="(movie, i) in movies" :key="i">
        <ul class="lista_film pb-5">
          <li>{{ movie.title }}</li>
          <li>{{ movie.original_title }}</li>
          <li>{{ movie.original_language }}</li>
          <li>{{ movie.vote_average }}</li>
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
      movies: [],
      userInput: "",
    };
  },

  methods: {
    movieCall() {
      if (!this.userInput) {
        return;
      }
      axios
        .get(`${this.apiUrl}/search/movie`, {
          params: {
            api_key: this.apiKey,
            query: this.userInput, //collega con v-model, è il titolo del fim
            languages: "en",
          },
        })
        .then((resp) => {
          this.movies = resp.data.results;
        });
    },
  },
  computed: {},
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
  /* .lista_film {
    padding-bottom: 20px;
  } */
}
</style>
