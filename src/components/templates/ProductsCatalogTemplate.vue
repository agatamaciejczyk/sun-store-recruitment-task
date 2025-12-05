<script lang="ts" setup>
import { ref, computed, watch, onMounted, onUnmounted } from "vue";

// components
import ProductCard from "@/components/atoms/ProductCard.vue";
import SearchBar from "@/components/atoms/SearchBar.vue";
import NoResults from "@/components/atoms/NoResults.vue";
import CategoryFilter from "@/components/atoms/CategoryFilter.vue";
import ManufacturerFilter from "@/components/atoms/ManufacturerFilter.vue";
import PriceRangeFilter from "@/components/atoms/PriceRangeFilter.vue";
import BasePagination from "@/components/atoms/BasePagination.vue";

// interfaces
import type { Product } from "@/interfaces/Product.ts";

interface Props {
  productsCatalog: Product[];
}

const props = defineProps<Props>();

const searchQuery = ref("");
const selectedCategory = ref("all");
const selectedManufacturer = ref("all");
const priceRange = ref<{
  min: number | null;
  max: number | null;
}>({
  min: null,
  max: null,
});
const currentPage = ref(1);
const itemsPerPage = 12;

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
  const priceMin = url.searchParams.get("priceMin")
    ? Number(url.searchParams.get("priceMin"))
    : null;
  const priceMax = url.searchParams.get("priceMax")
    ? Number(url.searchParams.get("priceMax"))
    : null;
  const page = url.searchParams.get("page")
    ? Number(url.searchParams.get("page"))
    : 1;

  return {
    search,
    category,
    manufacturer,
    priceRange: { min: priceMin, max: priceMax },
    page,
  };
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

  if (priceRange.value.min !== null) {
    url.searchParams.set("priceMin", priceRange.value.min.toString());
  } else {
    url.searchParams.delete("priceMin");
  }

  if (priceRange.value.max !== null) {
    url.searchParams.set("priceMax", priceRange.value.max.toString());
  } else {
    url.searchParams.delete("priceMax");
  }

  if (currentPage.value !== 1) {
    url.searchParams.set("page", currentPage.value.toString());
  } else {
    url.searchParams.delete("page");
  }

  window.history.pushState({}, "", url.toString());
};

onMounted(() => {
  const params = parseQueryParams();
  searchQuery.value = params.search;
  selectedCategory.value = params.category;
  selectedManufacturer.value = params.manufacturer;
  priceRange.value = params.priceRange;
  currentPage.value = params.page;

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
  priceRange.value = params.priceRange;
  currentPage.value = params.page;
};

watch([searchQuery, selectedCategory, selectedManufacturer, priceRange], () => {
  currentPage.value = 1;
}, { deep: true });

watch([searchQuery, selectedCategory, selectedManufacturer, priceRange, currentPage], () => {
  updateURL();
}, { deep: true });

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

  if (priceRange.value.min !== null) {
    products = products.filter(
      (product) => product.price >= priceRange.value.min!
    );
  }

  if (priceRange.value.max !== null) {
    products = products.filter(
      (product) => product.price <= priceRange.value.max!
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

  products.sort((a, b) => a.price - b.price);

  return products;
});

const totalPages = computed(() => {
  return Math.ceil(filteredProducts.value.length / itemsPerPage);
});

const paginatedProducts = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage;
  const end = start + itemsPerPage;
  return filteredProducts.value.slice(start, end);
});

const goToPage = (page: number) => {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page;
  }
};
</script>

<template>
  <div class="products-catalog">
    <div class="max-w-7xl mx-auto">
      <SearchBar
        v-model="searchQuery"
        placeholder="Search by name, manufacturer, or description..."
        :results-count="filteredProducts.length"
      />

      <div class="catalog-layout">
        <aside class="filters-sidebar">
          <CategoryFilter
            v-model="selectedCategory"
            :categories="categories"
          />

          <ManufacturerFilter
            v-model="selectedManufacturer"
            :manufacturers="manufacturers"
          />

          <PriceRangeFilter
            v-model="priceRange"
            :min-price="0"
            :max-price="1000000"
          />
        </aside>

        <div class="flex-1 min-w-0">
          <div
            v-if="filteredProducts.length > 0"
            class="products-catalog-grid"
          >
            <ProductCard
              v-for="(product, index) in paginatedProducts"
              :key="`product-card--${index}`"
              :product="product"
            />
          </div>

          <NoResults v-else />

          <BasePagination
            :current-page="currentPage"
            :total-pages="totalPages"
            @page-change="goToPage"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.products-catalog {
  @apply min-h-screen bg-gray-50 py-16 px-4;
}

.catalog-layout {
  @apply flex flex-col gap-6 lg:flex-row;
}

.filters-sidebar {
  @apply w-full flex-shrink-0 lg:w-80;
}

.products-catalog-grid {
  @apply grid grid-cols-1 gap-6 mb-8 md:grid-cols-2;
}
</style>
