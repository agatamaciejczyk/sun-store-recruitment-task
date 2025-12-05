<script lang="ts" setup>
import { ref, computed, watch, onMounted, onUnmounted } from "vue";

// components
import ProductCard from "@/components/atoms/ProductCard.vue";
import SearchBar from "@/components/atoms/SearchBar.vue";
import NoResults from "@/components/atoms/NoResults.vue";
import CategoryFilter from "@/components/atoms/CategoryFilter.vue";
import ManufacturerFilter from "@/components/atoms/ManufacturerFilter.vue";

// interfaces
import type { Product } from "@/interfaces/Product.ts";

interface Props {
  productsCatalog: Product[];
}

const props = defineProps<Props>();

const searchQuery = ref("");
const selectedCategory = ref("all");
const selectedManufacturer = ref("all");

const categories = computed(() => {
  return ["all", ...new Set(props.productsCatalog.map((p) => p.category))].sort(
    (a, b) => (a === "all" ? -1 : b === "all" ? 1 : a.localeCompare(b))
  );
});

const manufacturers = computed(() => {
  return [
    "all",
    ...new Set(props.productsCatalog.map((p) => p.manufacturer)),
  ].sort((a, b) => (a === "all" ? -1 : b === "all" ? 1 : a.localeCompare(b)));
});

const parseQueryParams = () => {
  const url = new URL(window.location.href);
  const search = url.searchParams.get("search") || "";
  const category = url.searchParams.get("category") || "all";
  const manufacturer = url.searchParams.get("manufacturer") || "all";

  return { search, category, manufacturer };
};

const updateURL = () => {
  const url = new URL(window.location.href);

  if (searchQuery.value.trim()) {
    url.searchParams.set("search", searchQuery.value);
  } else {
    url.searchParams.delete("search");
  }

  if (selectedCategory.value !== "all") {
    url.searchParams.set("category", selectedCategory.value);
  } else {
    url.searchParams.delete("category");
  }

  if (selectedManufacturer.value !== "all") {
    url.searchParams.set("manufacturer", selectedManufacturer.value);
  } else {
    url.searchParams.delete("manufacturer");
  }

  window.history.pushState({}, "", url.toString());
};

onMounted(() => {
  const params = parseQueryParams();
  searchQuery.value = params.search;
  selectedCategory.value = params.category;
  selectedManufacturer.value = params.manufacturer;

  window.addEventListener("popstate", handlePopState);
});

onUnmounted(() => {
  window.removeEventListener("popstate", handlePopState);
});

const handlePopState = () => {
  const params = parseQueryParams();
  searchQuery.value = params.search;
  selectedCategory.value = params.category;
  selectedManufacturer.value = params.manufacturer;
};

watch([searchQuery, selectedCategory, selectedManufacturer], () => {
  updateURL();
});

const filteredProducts = computed(() => {
  let products = props.productsCatalog;

  if (selectedCategory.value !== "all") {
    products = products.filter(
      (product) => product.category === selectedCategory.value
    );
  }

  if (selectedManufacturer.value !== "all") {
    products = products.filter(
      (product) => product.manufacturer === selectedManufacturer.value
    );
  }

  if (searchQuery.value.trim()) {
    const query = searchQuery.value.toLowerCase();
    products = products.filter(
      (product) =>
        product.name.toLowerCase().includes(query) ||
        product.manufacturer.toLowerCase().includes(query) ||
        product.description.toLowerCase().includes(query)
    );
  }

  return products;
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

      <CategoryFilter
        v-model="selectedCategory"
        :categories="categories"
      />

      <ManufacturerFilter
        v-model="selectedManufacturer"
        :manufacturers="manufacturers"
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
