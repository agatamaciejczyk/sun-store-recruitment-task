<script lang="ts" setup>
import { ref, watch } from "vue";

interface Props {
  minPrice: number;
  maxPrice: number;
  modelValue: {
    min: number | null;
    max: number | null;
  };
}

const props = defineProps<Props>();

const emit = defineEmits<{
  "update:modelValue": [value: { min: number | null; max: number | null }];
}>();

const localMin = ref<number | null>(props.modelValue.min);
const localMax = ref<number | null>(props.modelValue.max);

watch(
  () => props.modelValue,
  (newValue) => {
    localMin.value = newValue.min;
    localMax.value = newValue.max;
  },
  { deep: true }
);

watch([localMin, localMax], () => {
  emit("update:modelValue", {
    min: localMin.value,
    max: localMax.value,
  });
});

const resetFilter = () => {
  localMin.value = null;
  localMax.value = null;
};

const hasActiveFilter = () => {
  return localMin.value !== null || localMax.value !== null;
};
</script>

<template>
  <div class="price-range-filter">
    <div class="flex items-center justify-between mb-4">
      <h3 class="price-range-filter-title">Price Range</h3>
      <button
        v-if="hasActiveFilter()"
        @click="resetFilter"
        class="reset-button"
      >
        Reset
      </button>
    </div>
    <div class="price-inputs">
      <div class="price-input-group">
        <label for="price-min" class="price-label">Min</label>
        <input
          id="price-min"
          v-model.number="localMin"
          type="number"
          :min="props.minPrice"
          :max="localMax ?? props.maxPrice"
          class="price-input"
          placeholder="Min price"
        />
      </div>
      <span class="price-separator">-</span>
      <div class="price-input-group">
        <label for="price-max" class="price-label">Max</label>
        <input
          id="price-max"
          v-model.number="localMax"
          type="number"
          :min="localMin ?? props.minPrice"
          :max="props.maxPrice"
          class="price-input"
          placeholder="Max price"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.price-range-filter {
  @apply bg-white rounded-lg border border-gray-100 shadow-sm p-6 mb-6;
}

.price-range-filter-title {
  @apply text-lg font-semibold text-gray-900 mb-0;
}

.reset-button {
  @apply text-sm text-blue-600 hover:text-blue-700 font-medium transition-colors;
  @apply rounded px-2 py-1;
}

.price-inputs {
  @apply flex items-end gap-3;
}

.price-input-group {
  @apply flex flex-col gap-1 flex-1;
}

.price-label {
  @apply text-sm font-medium text-gray-700;
}

.price-input {
  @apply w-full px-4 py-2 text-sm border border-gray-200 rounded-lg;
  @apply transition-all duration-200;
}

.price-separator {
  @apply text-gray-400 pb-2 font-medium;
}
</style>
