<!-- App.vue -->
<template>
  <div class="min-h-screen bg-gradient-to-b from-gray-900 to-gray-800 text-gray-100 font-sans p-6">
    <div class="max-w-4xl mx-auto space-y-10 bg-gray-900 border border-gray-700 rounded-2xl shadow-xl p-6 sm:p-10">
      <h1 class="text-4xl sm:text-5xl font-extrabold text-center text-indigo-400 drop-shadow">
        🎬 Movie Watchlist
      </h1>

      <MovieSearch
        :searchQuery="searchQuery"
        @update-query="searchQuery = $event"
        @search="searchMovie"
      />

      <SearchResult
        v-if="searchResult"
        :searchResult="searchResult"
        @add="addToWatchlist"
      />

      <WantToWatch
        :movies="wantToWatch"
        @select="selectMovie"
        @remove="removeFromWatchlist"
      />

      <MovieDetails
        v-if="selectedMovie"
        :movie="selectedMovie"
        @watched="markAsWatched"
      />

      <button
        v-if="!showRecommendations && recommended.length"
        @click="showRecommendations = true"
        class="mt-4 text-sm text-indigo-400 hover:underline"
      >
        Show Recommendations
      </button>

      <Recommendation
        v-if="showRecommendations"
        :recommended="recommended"
        @hide="showRecommendations = false"
      />

      <WatchedList
        :movies="watched"
        @remove="removeFromWatched"
      />
    </div>
  </div>
</template>

<script>
import MovieSearch from './components/MovieSearch.vue';
import SearchResult from './components/SearchResult.vue';
import WantToWatch from './components/WantToWatch.vue';
import MovieDetails from './components/MovieDetails.vue';
import Recommendation from './components/Recommendation.vue';
import WatchedList from './components/WatchedList.vue';

export default {
  components: {
    MovieSearch,
    SearchResult,
    WantToWatch,
    MovieDetails,
    Recommendation,
    WatchedList,
  },
  data() {
    return {
      searchQuery: '',
      searchResult: null,
      wantToWatch: JSON.parse(localStorage.getItem('wantToWatch') || '[]'),
      watched: JSON.parse(localStorage.getItem('watched') || '[]'),
      selectedMovie: null,
      recommended: [],
      showRecommendations: false,
    };
  },
  mounted() {
    if (this.wantToWatch.length || this.watched.length) {
      this.generateRecommendations();
    }
  },
  methods: {
    async searchMovie(query) {
      const apiKey = import.meta.env.VITE_TMDB_API_KEY;

      const searchRes = await fetch(
        `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${encodeURIComponent(query)}`
      );

      const searchData = await searchRes.json();

      if (searchData.results && searchData.results.length > 0) {
        const movieId = searchData.results[0].id;

        const detailRes = await fetch(
          `https://api.themoviedb.org/3/movie/${movieId}?api_key=${apiKey}&language=en-US`
        );

        const movie = await detailRes.json();

        this.searchResult = {
          imdbID: movie.id,
          Title: movie.title,
          Year: movie.release_date?.slice(0, 4) ?? '',
          Genre: movie.genres.map((g) => g.name).join(', '),
          Plot: movie.overview,
          Poster: movie.poster_path
            ? `https://image.tmdb.org/t/p/w500${movie.poster_path}`
            : '',
        };
      } else {
        this.searchResult = null;
        alert('Movie not found.');
      }
    },
    addToWatchlist(movie) {
      if (!this.wantToWatch.find((m) => m.imdbID === movie.imdbID)) {
        this.wantToWatch.push(movie);
        this.save();
      }
    },
    selectMovie(movie) {
      this.selectedMovie = movie;
    },
    markAsWatched() {
      if (this.selectedMovie) {
        this.watched.push(this.selectedMovie);
        this.wantToWatch = this.wantToWatch.filter(
          (m) => m.imdbID !== this.selectedMovie.imdbID
        );
        this.selectedMovie = null;
        this.save();
        this.generateRecommendations();
      }
    },
    save() {
      localStorage.setItem('wantToWatch', JSON.stringify(this.wantToWatch));
      localStorage.setItem('watched', JSON.stringify(this.watched));
    },
    removeFromWatchlist(movie) {
      this.wantToWatch = this.wantToWatch.filter(m => m.imdbID !== movie.imdbID);
      this.save();
    },

    removeFromWatched(movie) {
      this.watched = this.watched.filter(m => m.imdbID !== movie.imdbID);

      // Only add to wantToWatch if it's not already there
      if (!this.wantToWatch.find(m => m.imdbID === movie.imdbID)) {
        this.wantToWatch.push(movie);
      }

      this.save();
    },

    async generateRecommendations() {
      const apiKey = import.meta.env.VITE_TMDB_API_KEY;
      const genreCount = {};

      const allMovies = [...this.watched, ...this.wantToWatch];
      allMovies.forEach((movie) => {
        if (movie.Genre) {
          movie.Genre.split(',').forEach((g) => {
            const genre = g.trim();
            genreCount[genre] = (genreCount[genre] || 0) + 1;
          });
        }
      });

      const topGenres = Object.entries(genreCount)
        .sort((a, b) => b[1] - a[1])
        .slice(0, 2)
        .map(([g]) => g);

      const genreRes = await fetch(
        `https://api.themoviedb.org/3/genre/movie/list?api_key=${apiKey}`
      );
      const genreData = await genreRes.json();
      const genreMap = {};
      genreData.genres.forEach((g) => (genreMap[g.name] = g.id));

      const genreIds = topGenres
        .map((name) => genreMap[name])
        .filter(Boolean)
        .join(',');

      if (!genreIds) return;

      const discoverRes = await fetch(
        `https://api.themoviedb.org/3/discover/movie?api_key=${apiKey}&with_genres=${genreIds}&sort_by=popularity.desc`
      );
      const discoverData = await discoverRes.json();

      this.recommended = discoverData.results.slice(0, 6).map((movie) => ({
        imdbID: movie.id,
        Title: movie.title,
        Year: movie.release_date?.slice(0, 4) ?? '',
        Genre: '',
        Plot: movie.overview,
        Poster: movie.poster_path
          ? `https://image.tmdb.org/t/p/w500${movie.poster_path}`
          : '',
      }));
      console.log(this.recommended);
    },
  },
};
</script>

<style scoped>
/* You can add component-scoped styles here if needed */
</style>
