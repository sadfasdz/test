<script>
import ModalFilmInfo from "./ModalFilmInfo.vue";

export default {
  data() {
    return {
      API_URL_SEARCH:
        "https://kinopoiskapiunofficial.tech/api/v2.1/films/search-by-keyword?keyword=",
      API_URL_POPULAR:
        "https://kinopoiskapiunofficial.tech/api/v2.2/films/top?type=TOP_250_BEST_FILMS&page=",
      page: 1,
      API_KEY: "8c8e1a50-6322-4135-8875-5d40a5420d86",
      arrMovies: [],
      isOpen: false,
      id: "",
    };
  },

  methods: {
    getMovies: async function (url, currentPage) {
      const response = await fetch(`${url}${currentPage}`, {
        headers: {
          "Content-Type": "application/json",
          "X-API-KEY": this.API_KEY,
        },
      });
      const responseData = await response.json();
      return (this.arrMovies = responseData.films);
    },

    closeModal: function () {
      this.isOpen = false;
    },

    openModal: function (filmId) {
      this.id = filmId;
      this.isOpen = true;
    },
  },
  beforeMount() {
    this.arrMovies = this.getMovies(this.API_URL_POPULAR, this.page);
  },
  computed: {},
  components: {
    ModalFilmInfo,
  },
};
</script>

<template>
  <main class="main">
    <div class="container">
      <section class="movies">
        <ul class="movies__list">
          <li
            @click="openModal(movie.filmId)"
            v-for="movie in this.arrMovies"
            class="movies__list-item"
          >
            <div class="movies__item-promo">
              <img
                :src="movie.posterUrlPreview"
                alt=""
                class="movies__item-img"
              />
              <div class="movies__item-phone"></div>
            </div>
            <div class="movies__item-promo">
              <h4 class="movies__item-title">{{ movie.nameRu }}</h4>
              <div class="movies__item-category">
                {{ movie.genres.map((genre) => genre.genre).join(", ") }}
              </div>
            </div>
          </li>
        </ul>
        <ModalFilmInfo v-if="isOpen" :id="this.id" @show="closeModal" />
        <!-- <div class="movies__pagination">
          <div
            class="movies__pagination-arrow movies__pagination-previous"
          ></div>
          <ul class="movies__pagination-list"></ul>
          <div class="movies__pagination-arrow movies__pagination-next">
            '>>'
          </div>
        </div> -->
      </section>
    </div>
  </main>
</template>

<style></style>
