<template>
  <div class="bg-gray-800 shadow p-5 rounded-md border border-gray-700">
    <h2 class="text-2xl font-bold mb-3 text-pink-400 border-b border-pink-600 pb-1">📌 Want to Watch</h2>
    <ul class="space-y-2 text-gray-200">
      <li
        v-for="movie in movies"
        :key="movie.imdbID"
        class="flex justify-between items-center"
      >
        <span
          class="cursor-pointer hover:text-blue-400"
          @click="$emit('select', movie)"
        >
          {{ movie.Title }} ({{ movie.Year }})
        </span>
        <button
          @click="$emit('remove', movie)"
          class="text-sm text-red-400 hover:text-red-600"
        >
          Remove from Watchlist
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["movies"],
};
</script>
