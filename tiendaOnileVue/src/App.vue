<template>
  <div class="min-h-screen flex flex-col items-center p-6 w-full">
    <h1 style="color: white;">Inventario de Productos</h1>
    <ProductList />
  </div>
</template>

<script setup lang="ts">
import ProductList from './components/ProductList.vue';
</script>