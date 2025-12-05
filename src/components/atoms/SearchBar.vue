<script lang="ts" setup>
import { computed } from "vue";

// icons
import { Search, X } from "lucide-vue-next";

interface Props {
  modelValue: string;
  resultsCount: number;
  placeholder?: string;
}

const props = withDefaults(defineProps<Props>(), {
  placeholder: "Search...",
});

const emit = defineEmits<{
  "update:modelValue": [value: string];
}>();

const localValue = computed({
  get: () => props.modelValue,
  set: (value: string) => emit("update:modelValue", value),
});

const clearSearch = () => {
  emit("update:modelValue", "");
};
</script>

<template>
  <div class="search-bar">
    <div class="search-wrapper">
      <Search class="search-icon" />
      <input
        v-model="localValue"
        type="text"
        :placeholder="placeholder"
        name="search"
        class="search-input"
      />
      <button
        v-if="localValue"
        @click="clearSearch"
        class="clear-button"
      >
        <X class="w-5 h-5" />
      </button>
    </div>
    <div v-if="localValue && resultsCount" class="search-results-info">
      Found {{ resultsCount }} product{{ resultsCount !== 1 ? "s" : "" }}
    </div>
  </div>
</template>

<style scoped>
.search-bar {
  @apply mb-8;
}

.search-wrapper {
  @apply relative flex items-center;
}

.search-icon {
  @apply absolute left-4 w-5 h-5 text-gray-400 pointer-events-none;
}

.search-input {
  @apply w-full pl-12 pr-12 py-4 text-gray-900 bg-white border border-gray-100 shadow-sm rounded-lg;
  @apply focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent;
  @apply placeholder:text-gray-400 transition-all duration-200;
}

.clear-button {
  @apply absolute right-3 p-1 text-gray-400 hover:text-gray-600 transition-colors duration-200;
  @apply focus:outline-none focus:ring-2 focus:ring-blue-500 rounded;
}

.search-results-info {
  @apply mt-3 text-sm text-gray-600 font-medium;
}
</style>
