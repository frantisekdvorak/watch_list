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
      <img v-if="movie.Poster" :src="movie.Poster" alt="Poster" class="w-full sm:w-40 rounded" />
      <div>
        <h2 class="text-xl font-semibold text-indigo-400">
          {{ movie.Title }} ({{ movie.Year }})
        </h2>
        <p class="text-sm text-gray-300 italic mb-2">{{ movie.Genre }}</p>
        <p class="text-gray-200 mb-2">{{ movie.Plot }}</p>

        <div class="text-sm text-gray-400 space-y-1 mt-2">
          <p>⭐ <span class="text-white font-medium">{{ movie.Rating }}</span> ({{ movie.Votes }} votes)</p>
          <p>🕒 Runtime: {{ movie.Runtime }} minutes</p>
          <p>🔥 Popularity: {{ movie.Popularity.toFixed(1) }}</p>
          <p>🌐 Language: {{ movie.Language.toUpperCase() }}</p>
        </div>

        <p v-if="movie.TrailerLink" class="mt-3">
          🎥 <a
            :href="movie.TrailerLink"
            target="_blank"
            class="text-indigo-400 hover:underline"
          >
            Watch Trailer on YouTube
          </a>
        </p>

        <button
          @click="$emit('watched')"
          class="mt-4 px-4 py-2 bg-green-600 hover:bg-green-500 text-white rounded"
        >
          Mark as Watched
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['movie'],
  emits: ['watched', 'close']
};
</script>
