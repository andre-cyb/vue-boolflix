<template>
  <div id="app">
    <nav class="navbar">
      <div>
        <h1 class="fw-bold text-danger ps-3 m-0">BOOLFLIX</h1>
      </div>

      <div class="input-group pe-3">
        <input
          class="form-control"
          type="search"
          v-model="userInput"
          placeholder="Cerca un contenuto"
          @keydown.enter="
            movieCall(`movie`, `movies`), movieCall(`tv`, `series`)
          "
        />
        <button
          type="button"
          class="btn btn-danger text-white"
          @click="movieCall(`movie`, `movies`), movieCall(`tv`, `series`)"
        >
          Search
        </button>
      </div>
    </nav>

    <div class="row">
      <ul>
        <h1 class="p-2 fw-bold" v-if="this.movies.length > 0">MOVIE</h1>
        <h1 class="fw-bold" style="font-size: 50px; padding-right: 60%" v-else>
          Scopri nuovi contenuti, tutti a portata di click!
        </h1>
        <FilmCard
          v-for="movie in movies"
          :key="movie.id"
          :titolo="movie.title"
          :titolo_originale="movie.original_title"
          :lingua_originale="movie.original_language"
          :type="movie"
          :trama="movie.overview"
        ></FilmCard>
        <!-- <li v-for="movie in movies" :key="movie.id">
          <div class="lista_film mb-5 position-relative">
            <img class="thumbnail" :src="thumb(movie)" alt="" />
            <div class="overlay">
              <h3>
                {{ movie.title }}
              </h3>
              <div
                :style="
                  movie.title === movie.original_title
                    ? `display: none`
                    : `display: block`
                "
              >
                {{ movie.original_title }}
              </div>
              <div>
                <img
                  class="m-2"
                  :src="lang[movie.original_language]"
                  width="30px"
                  alt=""
                />
              </div>
              <span
                >Voto:<i
                  class="fa fa-star"
                  aria-hidden="true"
                  v-for="(star, i) in voteFunction(movie)"
                  :key="i"
                ></i>
                <i
                  class="fa fa-star-o"
                  aria-hidden="true"
                  v-for="(star, i) in starEmpty(movie)"
                  :key="i + 'stella-vuota'"
                ></i>
              </span>
            </div>
          </div>
        </li> -->
      </ul>
      <ul>
        <h1 class="p-2 fw-bold" v-if="this.series.length > 0">TV SERIES</h1>
        <FilmCard
          v-for="serie in series"
          :key="serie.id"
          :titolo="serie.name"
          :titolo_originale="serie.original_name"
          :lingua_originale="serie.original_language"
          :type="serie"
          :trama="serie.overview"
        ></FilmCard>
        <!-- <li v-for="serie in series" :key="serie.id">
          <div class="lista_film mb-5 position-relative">
            <img class="thumbnail" :src="thumb(serie)" alt="" />
            <div class="overlay">
              <h3>
                {{ serie.name }}
              </h3>
              <div
                :style="
                  serie.name === serie.original_name
                    ? `display: none`
                    : `display: block`
                "
              >
                {{ serie.original_name }}
              </div>
              <div>
                <img
                  class="m-2"
                  :src="lang[serie.original_language]"
                  width="30px"
                  alt=""
                />
              </div>
              <span
                >Voto:<i
                  class="fa fa-star"
                  aria-hidden="true"
                  v-for="(star, i) in voteFunction(serie)"
                  :key="i + 'stella-piena'"
                ></i>
                <i
                  class="fa fa-star-o"
                  aria-hidden="true"
                  v-for="(star, i) in starEmpty(serie)"
                  :key="i + 'stella_vuota'"
                ></i
              ></span>
            </div>
          </div>
        </li> -->
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import FilmCard from "@/components/FilmCard.vue";

export default {
  name: "App",
  components: { FilmCard },
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
    flag(lang) {
      return this.lang[lang];
    },
    thumb(type) {
      let urlContent = "https://image.tmdb.org/t/p/";
      let idThumb = type.poster_path;
      if (!idThumb) {
        urlContent = require("@/assets/No_picture_available.png");
        return urlContent;
      }

      return urlContent + "w342" + idThumb;
    },
    voteFunction(type) {
      let vote = type.vote_average / 2;
      return Math.ceil(vote);
    },
    starEmpty(type) {
      return 5 - this.voteFunction(type);
    },
    /* titleOrName() {
      this;
    }, */
  },
  computed: {},
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
  color: #ffffff;
  background-color: #555555;
  height: 100vh;

  nav {
    background-color: rgb(24, 24, 24);
    height: 75px;
    .input-group {
      width: auto;
      input {
        &:focus {
          border-color: #dc3545;
        }
      }
      .btn-danger {
        box-shadow: none;
      }
      .btn {
        &:focus {
          box-shadow: none;
        }
      }
    }
  }
  .row {
    height: calc(100vh - 75px);
    overflow: auto;
    margin-right: 0;
  }
  ul {
    list-style: none;
    display: flex;
    flex-wrap: wrap;
    width: 90%;
    margin: auto;
    h1 {
      margin-top: 50px;
      width: 100%;
      text-align: start;
    }
  }
}

/* *
f
*/
</style>



 