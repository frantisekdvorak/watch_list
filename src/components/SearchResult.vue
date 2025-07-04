<template>
  <div class="bg-gray-800 border border-gray-700 shadow-md p-5 rounded-md text-center space-y-2">
    <p class="text-xl font-semibold text-gray-100">
      {{ searchResult.Title }} ({{ searchResult.Year }})
    </p>
    <button
      @click="$emit('add', searchResult)"
      class="bg-green-600 text-white px-4 py-2 rounded hover:bg-green-700 transition"
    >
      Add to Watchlist
    </button>
  </div>
</template>

<script>
export default {
  name: "SearchResult",
  props: {
    searchResult: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
</style>
