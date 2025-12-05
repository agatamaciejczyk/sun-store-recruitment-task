<script lang="ts" setup>
import { computed } from "vue";

// icons
import { ChevronLeftIcon, ChevronRightIcon } from "lucide-vue-next";

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

const visiblePages = computed(() => {
  const pages: (number | string)[] = [];
  const maxVisible = 5;

  if (props.totalPages <= maxVisible) {
    for (let i = 1; i <= props.totalPages; i++) {
      pages.push(i);
    }
  } else {
    pages.push(1);

    const start = Math.max(2, props.currentPage - 1);
    const end = Math.min(props.totalPages - 1, props.currentPage + 1);

    if (start > 2) {
      pages.push("...");
    }

    for (let i = start; i <= end; i++) {
      pages.push(i);
    }

    if (end < props.totalPages - 1) {
      pages.push("...");
    }

    pages.push(props.totalPages);
  }

  return pages;
});
</script>

<template>
  <div v-if="totalPages > 1" class="pagination">
    <button
      @click="goToPage(currentPage - 1)"
      :disabled="currentPage === 1"
      class="pagination-nav"
      aria-label="Previous page"
    >
      <ChevronLeftIcon />
    </button>

    <div class="pagination-pages">
      <template v-for="(page, index) in visiblePages" :key="index">
        <button
          v-if="typeof page === 'number'"
          @click="goToPage(page)"
          :class="[
            'pagination-page',
            { active: currentPage === page },
          ]"
        >
          {{ page }}
        </button>
        <span v-else class="pagination-ellipsis">{{ page }}</span>
      </template>
    </div>

    <button
      @click="goToPage(currentPage + 1)"
      :disabled="currentPage === totalPages"
      class="pagination-nav"
      aria-label="Next page"
    >
      <ChevronRightIcon />
    </button>
  </div>
</template>

<style scoped>
.pagination {
  @apply flex items-center justify-center gap-2 mt-8;
}

.pagination-nav {
  @apply w-10 h-10 rounded-lg text-lg font-medium transition-colors;
  @apply flex items-center justify-center;
  @apply bg-white text-gray-700 border border-gray-200;
  @apply hover:bg-gray-50 hover:border-gray-300;
  @apply disabled:opacity-50 disabled:cursor-not-allowed;
}

.pagination-pages {
  @apply flex gap-1;
}

.pagination-page {
  @apply w-8 h-8 md:w-10 md:h-10 rounded-lg text-xs md:text-sm font-medium transition-colors;
  @apply bg-white text-gray-700 border border-gray-200;
  @apply hover:bg-gray-50 hover:border-gray-300;
}

.pagination-page.active {
  @apply bg-blue-600 text-white border-blue-600;
  @apply hover:bg-blue-700 hover:border-blue-700;
}

.pagination-ellipsis {
  @apply w-8 h-8 md:w-10 md:h-10 flex items-center justify-center text-gray-400 text-xs md:text-sm;
}
</style>
