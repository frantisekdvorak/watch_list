<template>
  <div class="bg-gray-800 p-4 rounded shadow-md relative">
    <button
      @click="$emit('close')"
      class="absolute top-2 right-2 text-gray-400 hover:text-white text-sm"
      aria-label="Close"
    >
      ✖
    </button>

    <div class="flex flex-col sm:flex-row gap-4">
      <img v-if="searchResult.Poster" :src="searchResult.Poster" alt="Poster" class="w-full sm:w-32 rounded" />
      <div>
        <h2 class="text-xl font-bold text-indigo-400">
          {{ searchResult.Title }} ({{ searchResult.Year }})
        </h2>
        <p class="text-sm text-gray-300 italic mb-1">{{ searchResult.Genre }}</p>
        <p class="text-gray-200 mb-2">{{ searchResult.Plot }}</p>

        <div class="text-sm text-gray-400 space-y-1 mt-2">
          <p v-if="searchResult.Rating">⭐ <span class="text-white font-medium">{{ searchResult.Rating }}</span> ({{ searchResult.Votes }} votes)</p>
          <p v-if="searchResult.Runtime">🕒 Runtime: {{ searchResult.Runtime }} minutes</p>
          <p v-if="searchResult.Popularity">🔥 Popularity: {{ searchResult.Popularity.toFixed(1) }}</p>
          <p v-if="searchResult.Language">🌐 Language: {{ searchResult.Language.toUpperCase() }}</p>
        </div>

        <button
          @click="$emit('add', searchResult)"
          class="mt-4 px-4 py-2 bg-indigo-600 hover:bg-indigo-500 text-white rounded"
        >
          Add to Watchlist
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['searchResult'],
  emits: ['add', 'close']
};
</script>
