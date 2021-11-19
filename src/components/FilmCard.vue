<template>
  <li>
    <div class="lista_film mb-5 position-relative">
      <img class="thumbnail" :src="thumb(type)" alt="" />
      <div class="overlay">
        <h3>
          {{ titolo }}
        </h3>
        <div
          :style="
            titolo === titolo_originale ? `display: none` : `display: block`
          "
        >
          {{ titolo_originale }}
        </div>
        <div>
          <img class="m-2" :src="lang[lingua_originale]" width="30px" alt="" />
        </div>
        <span
          >Voto:<i
            class="fa fa-star"
            aria-hidden="true"
            v-for="(star, i) in voteFunction(type)"
            :key="i"
          ></i>
          <i
            class="fa fa-star-o"
            aria-hidden="true"
            v-for="(star, i) in starEmpty(type)"
            :key="i + 'stella-vuota'"
          ></i>
        </span>
        <p class="text-start pt-3" :class="!trama ? `d-none` : ``">
          <strong>Trama: </strong><br />
          {{ trama }}
        </p>
      </div>
    </div>
  </li>
</template>

<script>
export default {
  name: "FilmCard",
  props: ["titolo", "titolo_originale", "lingua_originale", "type", "trama"],
  data() {
    return {
      lang: {
        it: require("@/assets/it.svg"),
        en: require("@/assets/gb.svg"),
        fr: require("@/assets/fr.svg"),
        de: require("@/assets/de.svg"),
        es: require("@/assets/es.svg"),
      },
    };
  },
  methods: {
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
  },
};
</script>


<style scoped lang="scss">
li {
  list-style: none;
  width: calc(100% / 5);
  .thumbnail {
    height: 513px;
    object-fit: cover;
  }
  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    bottom: 0;
    opacity: 0;
    padding: 60px;
    background-color: rgba(0, 0, 0, 0.836);
    transition: all 0.2s linear;
    overflow: auto;
  }
  &:hover {
    .overlay {
      opacity: 1;
    }
  }
}
</style>
