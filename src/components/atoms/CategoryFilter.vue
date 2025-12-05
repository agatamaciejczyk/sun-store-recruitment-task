<script lang="ts" setup>
interface Props {
  categories: string[];
  modelValue: string;
}

const props = defineProps<Props>();

const emit = defineEmits<{
  "update:modelValue": [value: string];
}>();

const handleCategoryClick = (category: string) => {
  emit("update:modelValue", category);
};
</script>

<template>
  <div class="category-filter">
    <h3 class="category-filter-title">Categories</h3>
    <div class="flex flex-wrap gap-2">
      <button
        v-for="category in props.categories"
        :key="category"
        @click="handleCategoryClick(category)"
        :class="[
          'category-button',
          modelValue === category ? 'category-button-active' : '',
        ]"
      >
        {{ category === "all" ? "All Products" : category }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.category-filter {
  @apply bg-white rounded-lg border border-gray-100 shadow-sm p-6 mb-6;
}

.category-filter-title {
  @apply text-lg font-semibold text-gray-900 mb-4;
}

.category-button {
  @apply px-4 py-2 rounded-lg text-sm font-medium transition-all duration-200;
  @apply bg-gray-100 text-gray-700 border border-gray-200;
  @apply hover:bg-gray-200 hover:border-gray-300;
  @apply focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2;
}

.category-button-active {
  @apply bg-blue-600 text-white border-blue-600;
  @apply hover:bg-blue-700 hover:border-blue-700;
}
</style>
