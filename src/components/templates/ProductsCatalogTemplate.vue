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
    <pre>{{ props.productsCatalog }}</pre>
  </div>
</template>

<style lang="scss" scoped>
</style>
