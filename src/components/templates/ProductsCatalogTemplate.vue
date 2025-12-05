<script lang="ts" setup>
import { ref, computed } from "vue";

// components
import ProductCard from "@/components/atoms/ProductCard.vue";
import SearchBar from "@/components/atoms/SearchBar.vue";
import NoResults from '@/components/atoms/NoResults.vue';

// interfaces
import type { Product } from "@/interfaces/Product.ts";

interface Props {
  productsCatalog: Product[];
}

const props = defineProps<Props>();

const searchQuery = ref("");

const filteredProducts = computed(() => {
  if (!searchQuery.value.trim()) {
    return props.productsCatalog;
  }

  const query = searchQuery.value.toLowerCase();

  return props.productsCatalog.filter((product) => {
    return (
      product.name.toLowerCase().includes(query) ||
      product.manufacturer.toLowerCase().includes(query) ||
      product.description.toLowerCase().includes(query)
    );
  });
});
</script>

<template>
  <div class="products-catalog">
    <div class="max-w-7xl mx-auto">
      <SearchBar
        v-model="searchQuery"
        placeholder="Search by name, manufacturer, or description..."
        :results-count="filteredProducts.length"
      />

      <div
        v-if="filteredProducts.length > 0"
        class="products-catalog-grid"
      >
        <ProductCard
          v-for="(product, index) in filteredProducts"
          :key="`product-card--${index}`"
          :product="product"
        />
      </div>

      <NoResults v-else />
    </div>
  </div>
</template>

<style scoped>
.products-catalog {
  @apply min-h-screen bg-gray-50 py-16 px-4;
}

.products-catalog-grid {
  @apply grid grid-cols-1 md:grid-cols-2 gap-6;
}
</style>
