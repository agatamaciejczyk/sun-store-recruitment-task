<script lang="ts" setup>
// icons
import { Search } from 'lucide-vue-next'
</script>

<template>
  <div class="no-results">
    <Search class="no-results-icon" />
    <h3 class="no-results-title">No products found</h3>
    <p class="text-gray-600 max-w-md">
      Try adjusting your search terms or clearing the search filter.
    </p>
  </div>
</template>

<style scoped>
.no-results {
  @apply flex flex-col items-center justify-center py-16 text-center;
}

.no-results-icon {
  @apply w-16 h-16 text-gray-300 mb-4;
}

.no-results-title {
  @apply text-xl font-semibold text-gray-900 mb-2;
}
</style>
