<script lang="ts" setup>
// components
import ProductCard from "@/components/atoms/ProductCard.vue";

// interfaces
import type { Product } from "@/interfaces/Product.ts";

interface Props {
  productsCatalog: Product[];
}

const props = defineProps<Props>();
</script>

<template>
  <div class="products-catalog">
    <div class="max-w-7xl mx-auto">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <ProductCard
          v-for="(product, index) in props.productsCatalog"
          :key="`product-card--${index}`"
          :product="product"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.products-catalog {
  @apply min-h-screen bg-gray-50 py-16 px-4;
}
</style>
