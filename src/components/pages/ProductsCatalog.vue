<script lang="ts" setup>
import { onMounted, ref } from "vue";

// csv parser
import Papa from "papaparse";

// components
import ProductsCatalogTemplate from "@/components/templates/ProductsCatalogTemplate.vue";

// interfaces
import type { Product } from "@/interfaces/Product.ts";

const productsCatalog = ref<Product[]>([]);

onMounted(() => {
  Papa.parse('/products.csv', {
    download: true,
    header: true,
    complete: function(results) {
      productsCatalog.value = results.data;
    }
  });
});
</script>

<template>
  <ProductsCatalogTemplate :products-catalog="productsCatalog" />
</template>

<style lang="scss" scoped>
</style>
