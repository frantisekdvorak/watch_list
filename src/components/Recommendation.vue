<template>
  <div class="bg-gray-800 p-5 rounded-md border border-gray-700 shadow">
    <div class="flex items-center justify-between mb-3">
      <h2 class="text-2xl font-bold text-yellow-400">🎯 Recommended for You</h2>
      <button
        @click="$emit('hide')"
        class="text-sm text-gray-400 hover:text-gray-200 hover:underline"
      >
        Hide
      </button>
    </div>
    <ul class="space-y-3">
      <li
        v-for="movie in recommended"
        :key="movie.imdbID"
        class="flex gap-4 items-center"
      >
        <img
          :src="movie.Poster"
          class="w-16 rounded shadow border border-gray-600"
        />
        <div>
          <p class="text-lg font-semibold">{{ movie.Title }} ({{ movie.Year }})</p>
          <p class="text-sm text-gray-400">{{ movie.Genre }}</p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Recommendation',
  props: {
    recommended: Array,
  },
};
</script>

<style scoped>
/* Scoped styles if needed */
</style>
