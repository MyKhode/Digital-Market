<template>
    <div class="mx-auto max-w-screen-xl px-4 2xl:px-0">
      <div class="mb-4 items-end justify-between space-y-4 sm:flex sm:space-y-0 md:mb-8">
        <!-- Filters and other header components -->
        <h2 class="mt-3 text-xl font-semibold text-gray-900 dark:text-white sm:text-2xl">Electronics</h2>
      </div>
  
      <div class="mb-4 grid gap-4 sm:grid-cols-2 md:mb-8 lg:grid-cols-3 xl:grid-cols-4">
        <!-- Product Loop -->
        <div v-for="product in products" :key="product.id" class="rounded-lg border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-700 dark:bg-gray-800">
          <div class="h-56 w-full">
            <a :href="`/products/${product.id}`">
              <img class="mx-auto h-full dark:hidden" :src="product.imageUrl" alt="" />
              <img class="mx-auto hidden h-full dark:block" :src="product.imageUrlDark" alt="" />
            </a>
          </div>
          <div class="pt-6">
            <div class="mb-4 flex items-center justify-between gap-4">
              <span class="me-2 rounded bg-primary-100 px-2.5 py-0.5 text-xs font-medium text-primary-800 dark:bg-primary-900 dark:text-primary-300">
                {{ product.discount }}
              </span>
            </div>
            <a :href="`/products/${product.id}`" class="text-lg font-semibold leading-tight text-gray-900 hover:underline dark:text-white">{{ product.name }}</a>
            <div class="mt-2 flex items-center gap-2">
              <div class="flex items-center">
                <svg class="h-4 w-4 text-yellow-400" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M13.8 4.2a2 2 0 0 0-3.6 0L8.4 8.4l-4.6.3a2 2 0 0 0-1.1 3.5l3.5 3-1 4.4c-.5 1.7 1.4 3 2.9 2.1l3.9-2.3 3.9 2.3c1.5 1 3.4-.4 3-2.1l-1-4.4 3.4-3a2 2 0 0 0-1.1-3.5l-4.6-.3-1.8-4.2Z" />
                </svg>
              </div>
              <p class="text-sm font-medium text-gray-900 dark:text-white">{{ product.rating }}</p>
              <p class="text-sm font-medium text-gray-500 dark:text-gray-400">({{ product.reviews }})</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    data() {
      return {
        products: [], // Array to store product data
      };
    },
    created() {
      this.fetchProducts(); // Fetch products when the component is created
    },
    methods: {
      async fetchProducts() {
        try {
          const response = await axios.get('/api/products'); // Assuming this is your endpoint
          this.products = response.data; // Assign the response data to the products array
        } catch (error) {
          console.error("Error fetching products:", error);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  /* Add any styles if needed */
  </style>
  