<script lang="ts" setup>
interface Props {
  currentPage: number;
  totalPages: number;
}

const props = defineProps<Props>();

const emit = defineEmits<{
  "page-change": [page: number];
}>();

const goToPage = (page: number) => {
  if (page >= 1 && page <= props.totalPages) {
    emit("page-change", page);
  }
};
</script>

<template>
  <div v-if="totalPages > 1" class="base-pagination">
    <button
      @click="goToPage(currentPage - 1)"
      :disabled="currentPage === 1"
      class="base-pagination-button"
    >
      Previous
    </button>

    <div class="flex gap-1">
      <button
        v-for="page in totalPages"
        :key="page"
        @click="goToPage(page)"
        :class="[
          'base-pagination-page',
          currentPage === page ? 'base-pagination-page-active' : '',
        ]"
      >
        {{ page }}
      </button>
    </div>

    <button
      @click="goToPage(currentPage + 1)"
      :disabled="currentPage === totalPages"
      class="base-pagination-button"
    >
      Next
    </button>
  </div>
</template>

<style scoped>
.base-pagination {
  @apply flex items-center justify-center gap-2 mt-8;
}

.base-pagination-button {
  @apply px-4 py-2 rounded-lg text-sm font-medium transition-all duration-200;
  @apply bg-white text-gray-700 border border-gray-200;
  @apply hover:bg-gray-50 hover:border-gray-300;
  @apply focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2;
  @apply disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:bg-white;
}

.base-pagination-page {
  @apply w-10 h-10 rounded-lg text-sm font-medium transition-all duration-200;
  @apply bg-white text-gray-700 border border-gray-200;
  @apply hover:bg-gray-50 hover:border-gray-300;
  @apply focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2;
}

.base-pagination-page-active {
  @apply bg-blue-600 text-white border-blue-600;
  @apply hover:bg-blue-700 hover:border-blue-700;
}
</style>
