<script setup lang="ts">
import { supabase } from "@/services/supabase";
import PromoteSection from "@/components/PromoteSection.vue";
import ProductCard from "@/components/ProductCard.vue";
</script>

<template>
  <div>
    <PromoteSection />
    <ProductCard />
    <!-- <ProductCard /> -->
  </div>

  <!-- <h1 class="text-3xl font-medium">Home</h1> -->
  <!-- <pre>{{ supabase.auth.user() }}</pre> -->
</template>
