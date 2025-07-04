<template>
  <div class="bg-gray-800 shadow p-5 rounded-md border border-gray-700">
    <h2 class="text-2xl font-bold mb-3 text-pink-400 border-b border-pink-600 pb-1">
      📌 Want to Watch
    </h2>
    <ul class="list-disc pl-5 space-y-1 text-gray-200">
      <li
        v-for="movie in movies"
        :key="movie.imdbID"
        class="cursor-pointer hover:text-blue-400"
        @click="$emit('select', movie)"
      >
        {{ movie.Title }} ({{ movie.Year }})
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "WantToWatch",
  props: {
    movies: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style scoped>
</style>
