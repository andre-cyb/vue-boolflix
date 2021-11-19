<template>
  <ul>
    <!-- <li>
      <div class="lista_film pb-5">
        <img :src="thumb(movie)" alt="" />
        <h3>
          {{ data.title }}
        </h3>
        <div>{{ data.original_title }}</div>
        <div>
          <img :src="lang[data.original_language]" width="30px" alt="" />
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
    </li> -->
  </ul>
</template>

<script>
export default {
  name: "FilmCard",
  props: {
    data: Object,
  },
  data() {
    return {
      lang: {
        it: require("@/assets/it.svg"),
        en: require("@/assets/gb.svg"),
        fr: require("@/assets/fr.svg"),
        de: require("@/assets/de.svg"),
      },
    };
  },
};
</script>


<style scoped lang="scss">
ul {
  list-style: none;
  width: 50%;
  li {
    list-style: none;
  }
}
</style>
