<script lang="ts" setup>
// interfaces
import type { Product } from "@/interfaces/Product.ts";

interface Props {
  productsCatalog: Product[];
}

const props = defineProps<Props>();
</script>

<template>
  <div class="products-catalog">
    test
<!--    <pre>{{ props.productsCatalog }}</pre>-->
  </div>
</template>

<style scoped>
.products-catalog {
  @apply bg-blue-900;
}
</style>
