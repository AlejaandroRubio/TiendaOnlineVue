<template>
    <div>
      <ProductItem 
        v-for="(product, index) in products" 
        :key="index" 
        :product="product"
        :index="index"
        @update-stock="updateStock"
      />
    </div>
  </template>
  
  <script setup lang="ts">
  import { reactive, watch } from 'vue';
  import ProductItem from './ProductItem.vue';
  import type { Product } from '../types/Product';
  
  // Definir productos reactivos
  const products = reactive<Product[]>([
    { nombre: 'Laptop', precio: 1000, stock: 5, disponible: true },
    { nombre: 'Mouse', precio: 50, stock: 0, disponible: false },
    { nombre: 'Teclado', precio: 80, stock: 3, disponible: true }
  ]);
  
  // Watch para actualizar 'disponible' cuando cambie 'stock'
  watch(products, (newProducts) => {
    newProducts.forEach(product => {
      product.disponible = product.stock > 0;
    });
  }, { deep: true });
  
  // Función para modificar el stock
  const updateStock = (index: number, amount: number) => {
    products[index].stock += amount;
  };
  </script>
  