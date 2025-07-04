<template>
  <div class="relative">
    <input
      v-model="localQuery"
      @input="onInput"
      @keydown.down.prevent="moveSelection(1)"
      @keydown.up.prevent="moveSelection(-1)"
      @keydown.enter.prevent="selectSuggestion()"
      placeholder="Search for a movie..."
      class="w-full px-4 py-2 bg-gray-800 text-white border border-indigo-500 rounded focus:outline-none focus:ring focus:ring-indigo-300"
    />

    <ul v-if="suggestions.length" class="bg-gray-800 text-white border border-gray-600 rounded mt-2 shadow-lg">
      <li
        v-for="(movie, index) in suggestions"
        :key="movie.id"
        @click="selectSuggestion(index)"
        :class="[
          'cursor-pointer px-4 py-2',
          selectedIndex === index ? 'bg-indigo-600' : 'hover:bg-indigo-500'
        ]"
      >
        {{ movie.title }} ({{ movie.release_date?.slice(0, 4) || "N/A" }})
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ['searchQuery'],
  emits: ['update-query', 'search'],
  data() {
    return {
      localQuery: this.searchQuery,
      suggestions: [],
      selectedIndex: -1,
    };
  },
  watch: {
    searchQuery(newVal) {
      this.localQuery = newVal;
    },
  },
  methods: {
    async onInput() {
      this.$emit('update-query', this.localQuery);

      if (this.localQuery.length < 2) {
        this.suggestions = [];
        return;
      }

      const apiKey = import.meta.env.VITE_TMDB_API_KEY;
      const res = await fetch(
        `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${encodeURIComponent(this.localQuery)}`
      );
      const data = await res.json();
      this.suggestions = data.results?.slice(0, 5) || [];
      this.selectedIndex = -1;
    },
    moveSelection(step) {
      const count = this.suggestions.length;
      if (!count) return;
      this.selectedIndex = (this.selectedIndex + step + count) % count;
    },
    selectSuggestion(index = this.selectedIndex) {
      const movie = index >= 0 ? this.suggestions[index] : null;
      if (movie) {
        this.localQuery = movie.title;
        this.$emit('update-query', movie.title);
        this.$emit('search', movie.title);
        this.suggestions = [];
        this.selectedIndex = -1;
      }
    },
  },
};
</script>

<style scoped>
input:focus {
  outline: none;
  border-color: #6366f1;
  box-shadow: 0 0 0 1px #6366f1;
}
</style>
