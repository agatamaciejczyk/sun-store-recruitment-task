<script lang="ts" setup>
// interfaces
import type { Product } from "@/interfaces/Product.ts";

interface Props {
  product: Product;
}

const props = defineProps<Props>();
</script>

<template>
  <article class="product-card">
    <div class="product-card-main">
      <div class="product-card-content-wrapper">
        <div class="flex flex-col gap-1">
          <div class="product-header">
            <h2 class="product-name">{{ props.product.name }}</h2>
            <span class="product-category">{{ props.product.category }}</span>
          </div>

          <div class="product-manufacturer">
            <span>Manufacturer:</span>
            <span>{{ props.product.manufacturer }}</span>
          </div>
        </div>

        <p class="text-gray-600 leading-relaxed">{{ props.product.description }}</p>
      </div>

      <div class="flex justify-end mt-4">
        <span class="text-lg font-bold text-gray-900">${{ props.product.price }}</span>
      </div>
    </div>
  </article>
</template>

<style scoped>
.product-card {
  @apply bg-white rounded-lg shadow-sm hover:shadow-md transition-all duration-300 overflow-hidden border border-gray-100 hover:border-gray-200;
}

.product-card-main {
  @apply flex flex-col p-8 h-full;
}

.product-card-content-wrapper {
  @apply flex flex-col gap-4 flex-grow;
}

.product-header {
  @apply flex flex-wrap items-start gap-3;
}

.product-name {
  @apply text-xl font-bold text-gray-900 flex-1 min-w-0 transition-colors duration-200 md:text-2xl;
}

.product-category {
  @apply inline-flex items-center px-3 py-1 rounded-full text-xs font-medium bg-blue-100 text-blue-800 border border-blue-200;
}

.product-manufacturer {
  @apply flex items-center gap-1 text-sm text-gray-400 font-semibold;
}
</style>
