<template>
  <section>
    <h1 class="page__title">Movies</h1>
    <p class="page__subtitle">카테고리별로 다양한 영화 정보를 보여드릴게요</p>

    <h1 class="movielist__title">최신 영화</h1>
    <MovieCarousel :movies="nowplaying_MovieList" />

    <h1 class="movielist__title">인기 영화</h1>
    <MovieCarousel :movies="popular_MovieList" />

    <h1 class="movielist__title">역대 인기 영화</h1>
    <MovieCarousel :movies="toprated_MovieList" />

    <h1 class="movielist__title">개봉 예정 영화</h1>
    <MovieCarousel :movies="upcoming_MovieList" />

    <h1 class="movielist__title">모든 영화</h1>
    <MovieCarousel :movies="MovieList" />
  </section>
</template>

<script lang="ts">
import Vue from "vue";

import axios from "axios";
import MovieCarousel from "@/components/MovieCarousel.vue";

const MOVIE_DB_API_URL_POPULAR = "https://api.themoviedb.org/3/movie/popular";
const MOVIE_DB_API_URL_UPCOMING = "https://api.themoviedb.org/3/movie/upcoming";
const MOVIE_DB_API_URL_TOP_RATED =
  "https://api.themoviedb.org/3/movie/top_rated";
const MOVIE_DB_API_URL_GET_NOW_PLAYING =
  "https://api.themoviedb.org/3/movie/now_playing";

export default Vue.extend({
  name: "Home",
  data() {
    return {
      popular_MovieList: [],
      upcoming_MovieList: [],
      toprated_MovieList: [],
      nowplaying_MovieList: [],
      MovieList: [] as any,
    };
  },
  props: {},
  components: {
    MovieCarousel,
  },
  created() {
    axios
      .get(MOVIE_DB_API_URL_POPULAR, {
        params: {
          api_key: process.env.VUE_APP_TMDB_API_KEY,
          language: "ko-KR",
          page: 1,
          region: "kr",
        },
      })
      .then((res) => {
        this.popular_MovieList = res.data.results;
      })
      .catch((err) => console.log(err));
    axios
      .get(MOVIE_DB_API_URL_UPCOMING, {
        params: {
          api_key: process.env.VUE_APP_TMDB_API_KEY,
          language: "ko-KR",
          page: 1,
          region: "kr",
        },
      })
      .then((res) => {
        this.upcoming_MovieList = res.data.results;
      })
      .catch((err) => console.log(err));
    axios
      .get(MOVIE_DB_API_URL_TOP_RATED, {
        params: {
          api_key: process.env.VUE_APP_TMDB_API_KEY,
          language: "ko-KR",
          page: 1,
          region: "kr",
        },
      })
      .then((res) => {
        this.toprated_MovieList = res.data.results;
      })
      .catch((err) => console.log(err));
    axios
      .get(MOVIE_DB_API_URL_GET_NOW_PLAYING, {
        params: {
          api_key: process.env.VUE_APP_TMDB_API_KEY,
          language: "ko-KR",
          page: 1,
          region: "kr",
        },
      })
      .then((res) => {
        this.nowplaying_MovieList = res.data.results;
      })
      .catch((err) => console.log(err));

    axios
      .all([
        axios.get(MOVIE_DB_API_URL_GET_NOW_PLAYING, {
          params: {
            api_key: process.env.VUE_APP_TMDB_API_KEY,
            language: "ko-KR",
            page: 1,
            region: "kr",
          },
        }),
        axios.get(MOVIE_DB_API_URL_GET_NOW_PLAYING, {
          params: {
            api_key: process.env.VUE_APP_TMDB_API_KEY,
            language: "ko-KR",
            page: 2,
            region: "kr",
          },
        }),
        axios.get(MOVIE_DB_API_URL_GET_NOW_PLAYING, {
          params: {
            api_key: process.env.VUE_APP_TMDB_API_KEY,
            language: "ko-KR",
            page: 3,
            region: "kr",
          },
        }),
        axios.get(MOVIE_DB_API_URL_GET_NOW_PLAYING, {
          params: {
            api_key: process.env.VUE_APP_TMDB_API_KEY,
            language: "ko-KR",
            page: 4,
            region: "kr",
          },
        }),
      ])
      .then(
        axios.spread((...responses) => {
          this.MovieList.push(...responses[0].data.results);
          this.MovieList.push(...responses[1].data.results);
          this.MovieList.push(...responses[2].data.results);
          this.MovieList.push(...responses[3].data.results);
        })
      )
      .catch((err) => console.log(err));
  },
  methods: {},
});
</script>

<style scoped>
.page__title {
  font-family: "Mulish", sans-serif;
  letter-spacing: 0rem;
  color: var(--page-title);
  font-size: 20px;
  font-weight: 700;
}

.page__subtitle {
  font-size: 14px;
  color: var(--profile-settings-description);
}

.movielist__title {
  font-size: 24px;
  font-weight: 700;
  margin-top: 3rem;
  margin-bottom: 0.5rem;
}
</style>
