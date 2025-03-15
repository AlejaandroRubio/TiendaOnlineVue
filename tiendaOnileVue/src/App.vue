<template>
  <div class="min-h-screen bg-white flex flex-col items-center p-6 w-full">
    <h1 class="text-3xl font-bold text-gray-800 mb-6">Inventario de Productos</h1>
    <ProductList />
  </div>
</template>

<script setup lang="ts">
import ProductList from './components/ProductList.vue';
</script>
