<template>
  <div class="bg-gray-800 shadow p-5 rounded-md border border-gray-700">
    <h2 class="text-2xl font-bold mb-3 text-green-400 border-b border-green-600 pb-1">
      ✅ Watched
    </h2>
    <ul class="list-disc pl-5 space-y-1 text-gray-300">
      <li v-for="movie in movies" :key="movie.imdbID">
        {{ movie.Title }} ({{ movie.Year }})
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'WatchedList',
  props: {
    movies: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style scoped>
/* Add additional styling if needed */
</style>
