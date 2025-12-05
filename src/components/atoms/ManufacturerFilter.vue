<script lang="ts" setup>
interface Props {
  manufacturers: string[];
  modelValue: string;
}

const props = defineProps<Props>();

const emit = defineEmits<{
  "update:modelValue": [value: string];
}>();

const handleManufacturerClick = (manufacturer: string) => {
  emit("update:modelValue", manufacturer);
};
</script>

<template>
  <div class="manufacturer-filter">
    <h3 class="manufacturer-filter-title">Manufacturers</h3>
    <div class="flex flex-wrap gap-2">
      <button
        v-for="manufacturer in props.manufacturers"
        :key="manufacturer"
        @click="handleManufacturerClick(manufacturer)"
        :class="[
          'manufacturer-button',
          modelValue === manufacturer ? 'manufacturer-button-active' : '',
        ]"
      >
        {{ manufacturer === "all" ? "All Manufacturers" : manufacturer }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.manufacturer-filter {
  @apply bg-white rounded-lg border border-gray-100 shadow-sm p-6 mb-6;
}

.manufacturer-filter-title {
  @apply text-lg font-semibold text-gray-900 mb-4;
}

.manufacturer-button {
  @apply px-4 py-2 rounded-lg text-sm font-medium transition-all duration-200;
  @apply bg-gray-100 text-gray-700 border border-gray-200;
  @apply hover:bg-gray-200 hover:border-gray-300;
  @apply focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2;
}

.manufacturer-button-active {
  @apply bg-blue-600 text-white border-blue-600;
  @apply hover:bg-blue-700 hover:border-blue-700;
}
</style>
