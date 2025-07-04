<template>
  <div class="flex flex-col sm:flex-row gap-3 items-center justify-center">
    <input
      :value="searchQuery"
      @input="$emit('update-query', $event.target.value)"
      placeholder="Search a movie..."
      class="w-full sm:w-2/3 px-4 py-3 bg-gray-800 border border-gray-600 rounded-md text-lg focus:outline-none focus:ring-2 focus:ring-indigo-500"
      @keyup.enter="$emit('search', searchQuery)"
    />
    <button
      @click="$emit('search', searchQuery)"
      class="bg-indigo-600 text-white px-5 py-3 rounded-md shadow hover:bg-indigo-700 transition"
    >
      Search
    </button>
  </div>
</template>

<script>
export default {
  name: 'MovieSearch',
  props: {
    searchQuery: {
      type: String,
      required: true
    }
  }
};
</script>

<style scoped>
/* Optional scoped styles */
</style>
