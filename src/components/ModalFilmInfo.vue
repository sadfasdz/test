<script>
export default {
  props: ["id"],
  emits: ["show"],
  data() {
    return {
      API_KEY: "8c8e1a50-6322-4135-8875-5d40a5420d86",
      API_URL_FILM_DELAILS:
        "https://kinopoiskapiunofficial.tech/api/v2.2/films/",
      filmDetails: [],
    };
  },

  methods: {
    getFilmDetails: async function (url, id) {
      const response = await fetch(`${url}${id}`, {
        headers: {
          "Content-Type": "application/json",
          "X-API-KEY": this.API_KEY,
        },
      });
      const responseData = await response.json();
      return (this.filmDetails = responseData);
    },

    getGenre: function (genreObj) {
      // genreObj.genre.map((genre) => genre.genre)
      console.log(genreObj);
    },
  },

  beforeMount() {
    this.filmDetails = this.getFilmDetails(this.API_URL_FILM_DELAILS, this.id);
  },
};
</script>

<template>
  <div class="modal">
    <div class="modal__card" :class="(modal__show = isOpen)">
      <img class="modal__movie-backdrop" :src="filmDetails.posterUrl" />
      <h2>
        <span class="modal__movie-title">{{ filmDetails.nameRu }}</span>
        <span class="modal__movie-release-year">{{ filmDetails.year }}</span>
      </h2>
      <ul class="modal__movie-info">
        <div class="loader"></div>
        <li class="modal__movie-genre">
          <!-- {{ filmDetails.genres }} -->
          <!-- {{ filmDetails.genres.map((genre) => genre.genre).join(", ") }} -->
          {{ getGenre(filmDetails.genres) }}
        </li>
        <li class="modal__movie-genre"></li>
        <li class="modal__movie-runtime">{{ filmDetails.filmLength }}</li>
        <li>
          Сайт
          <a :href="filmDetails.webUrl" class="modal__movie-site">{{
            filmDetails.webUrl
          }}</a>
        </li>
        <li class="modal__movie-overview">{{ filmDetails.description }}</li>
      </ul>
      <button @click="$emit('show')" class="modal__button-close">
        Закрыть
      </button>
    </div>
  </div>
</template>

<style lang=""></style>
