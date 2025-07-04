<template>
  <div class="bg-gray-800 shadow p-5 rounded-md border border-gray-700">
    <h2 class="text-2xl font-bold mb-3 text-green-400 border-b border-green-600 pb-1">✅ Watched</h2>
    <ul class="space-y-2 text-gray-300">
      <li
        v-for="movie in movies"
        :key="movie.imdbID"
        class="flex justify-between items-center"
      >
        <span>{{ movie.Title }} ({{ movie.Year }})</span>
        <button
          @click="$emit('remove', movie)"
          class="ml-4 text-sm text-yellow-400 hover:underline"
        >
          Add Back to Watchlist
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
