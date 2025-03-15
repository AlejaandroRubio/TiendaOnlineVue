<template>
  <div class="grid grid-cols-3 gap-6 w-full p-4">
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

const products = reactive<Product[]>([
  { nombre: 'Laptop', precio: 1000, stock: 5, disponible: true, imagen: '/images/laptop.jpg' },
  { nombre: 'Mouse', precio: 50, stock: 0, disponible: false, imagen: '/images/mouse.jpg' },
  { nombre: 'Teclado', precio: 80, stock: 3, disponible: true, imagen: '/images/teclado.jpg' },
  { nombre: 'Teclado1', precio: 80, stock: 3, disponible: true, imagen: '/images/teclado.jpg' },
  { nombre: 'Teclado2', precio: 80, stock: 3, disponible: true, imagen: '/images/teclado.jpg' },
  { nombre: 'Teclado3', precio: 80, stock: 3, disponible: true, imagen: '/images/teclado.jpg' },
  { nombre: 'Teclado4', precio: 80, stock: 3, disponible: true, imagen: '/images/teclado.jpg' }
]);

watch(products, (newProducts) => {
  newProducts.forEach(product => {
    product.disponible = product.stock > 0;
  });
}, { deep: true });

const updateStock = (index: number, amount: number) => {
  products[index].stock += amount;
};
</script>
