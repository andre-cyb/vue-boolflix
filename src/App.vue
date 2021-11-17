<template>
  <div id="app">
    <input type="search" v-model="userInput" />
    <button @click="submitSearch()">Search</button>
    <ul>
      <li v-for="(movie, i) in movieCall()" :key="i">{{ movie.title }}</li>
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
      test: "",
    };
  },

  methods: {
    movieCall() {
      axios
        .get(`${this.apiUrl}/search/movie`, {
          params: {
            api_key: this.apiKey,
            query: this.test, //collega con v-model, è il titolo del fim
            languages: "en",
          },
        })
        .then((resp) => {
          this.movies = resp.data.results;
        });
      return this.movies;
    },
    submitSearch() {
      this.test = this.userInput;
      return this.test;
    },
  },
  computed: {},
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
