<template>
  <div class="bg-gray-800 shadow-lg p-6 rounded-xl space-y-4 text-center border border-gray-700">
    <h3 class="text-2xl font-semibold text-indigo-300">{{ movie.Title }}</h3>
    <img
      :src="movie.Poster"
      alt="Poster"
      class="w-44 mx-auto rounded-md border border-gray-500"
    />
    <p class="text-sm text-gray-400 leading-relaxed">{{ movie.Plot }}</p>
    <button
      @click="$emit('watched')"
      class="bg-purple-600 text-white px-5 py-2 rounded-lg hover:bg-purple-700 transition"
    >
      Mark as Watched
    </button>
  </div>
</template>

<script>
export default {
  name: 'MovieDetails',
  props: {
    movie: Object
  }
};
</script>
