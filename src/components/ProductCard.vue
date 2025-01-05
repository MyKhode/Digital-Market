<script setup>
import { ref, reactive, onMounted } from 'vue';
import { createClient } from '@supabase/supabase-js';

// Replace with your actual Supabase URL and public key
const supabaseUrl = import.meta.env.VITE_SUPABASE_URL;
const supabaseKey = import.meta.env.VITE_SUPABASE_KEY;
const supabase = createClient(supabaseUrl, supabaseKey);

// Reactive reference to hold fetched product data
const products = ref([]);

// Reactive object for displaying product details
const selectedProduct = reactive({
  id: null,
  name: '',
  description: '',
  price: null,
  screenshotimage: '',
  created: '',
  show: false,
});

// Fetch data from Supabase when the component is mounted
onMounted(async () => {
  const { data, error } = await supabase.from('products').select('*');
  if (error) {
    console.error('Error fetching products:', error);
  } else {
    products.value = data;
  }
});

// Function to fetch and display product details
const showDetails = async (productId) => {
  const { data, error } = await supabase
    .from('products')
    .select('*')
    .eq('id', productId)
    .single();
  if (error) {
    console.error('Error fetching product details:', error);
  } else {
    Object.assign(selectedProduct, data, { show: true });
  }
};

// Function to close the details modal
const closeDetails = () => {
  selectedProduct.show = false;
};
</script>

<template>
  <div class="mx-auto max-w-screen-xl px-4 2xl:px-0">
    <!-- Product Grid -->
    <div class="mb-4 grid gap-4 sm:grid-cols-2 md:mb-8 lg:grid-cols-3 xl:grid-cols-4">
      <div
        v-for="product in products"
        :key="product.id"
        class="rounded-lg border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-700 dark:bg-gray-800"
      >
        <div class="h-56 w-full">
          <a href="#">
            <img
              class="mx-auto h-full dark:hidden"
              :src="product.thumbnail"
              alt="Product image"
            />
            <img
              class="mx-auto hidden h-full dark:block"
              :src="product.thumbnail"
              alt="Product image dark mode"
            />
          </a>
        </div>
        <div class="pt-6">
          <a href="#" class="text-lg font-semibold leading-tight text-gray-900 hover:underline dark:text-white">
            {{ product.name }}
          </a>
          <p class="mt-2 text-sm font-medium text-gray-500 dark:text-gray-400">{{ product.description }}</p>
          <p class="mt-4 text-lg font-bold text-gray-900 dark:text-white">${{ product.price }}</p>
          <div class="mt-4 flex gap-4">
            <button
              class="rounded bg-primary-600 px-4 py-2 text-sm font-medium text-white hover:bg-primary-700 dark:bg-primary-500 dark:hover:bg-primary-600"
              @click="showDetails(product.id)"
            >
              View Details
            </button>
            <button
              class="rounded bg-green-600 px-4 py-2 text-sm font-medium text-white hover:bg-green-700 dark:bg-green-500 dark:hover:bg-green-600"
            >
              Buy Now
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Product Details Modal -->
    <div
      v-if="selectedProduct.show"
      class="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-50"
    >
      <div class="w-full max-w-lg rounded-lg bg-white p-6 shadow-lg dark:bg-gray-800">
        <h2 class="mb-4 text-lg font-bold text-gray-900 dark:text-white">
          {{ selectedProduct.name }}
        </h2>
        <p class="mb-4 text-sm text-gray-500 dark:text-gray-400">
          {{ selectedProduct.description }}
        </p>
        <img
          v-if="selectedProduct.screenshotimage"
          :src="selectedProduct.screenshotimage"
          alt="Screenshot"
          class="mb-4 w-full rounded"
        />
        <p class="text-sm font-medium text-gray-700 dark:text-gray-300">
          <strong>Price:</strong> ${{ selectedProduct.price }}
        </p>
        <p class="text-sm font-medium text-gray-700 dark:text-gray-300">
          <strong>Created At:</strong> {{ new Date(selectedProduct.created).toLocaleDateString() }}
        </p>
        <div class="mt-6 flex justify-end">
          <button
            class="rounded bg-gray-600 px-4 py-2 text-sm font-medium text-white hover:bg-gray-700 dark:bg-gray-500 dark:hover:bg-gray-600"
            @click="closeDetails"
          >
            Close
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
