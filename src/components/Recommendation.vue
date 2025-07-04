<template>
  <div class="grid gap-4 sm:grid-cols-2 md:grid-cols-3">
    <div
      v-for="movie in recommended"
      :key="movie.imdbID"
      class="bg-gray-800 p-4 rounded-lg shadow hover:shadow-lg transition cursor-pointer"
      @click="addMovie(movie)"
    >
      <img
        :src="movie.Poster"
        :alt="movie.Title"
        class="w-full h-60 object-cover rounded mb-2"
      />
      <h3 class="text-lg font-semibold">{{ movie.Title }}</h3>
      <p class="text-sm text-gray-400">{{ movie.Year }}</p>
      <p class="text-xs mt-1 text-gray-300 line-clamp-3">{{ movie.Plot }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['recommended'],
  emits: ['add'],
  methods: {
    addMovie(movie) {
      this.$emit('add', movie); // This will trigger App.vue's `addToWatchlist`
    },
  },
};
</script>
