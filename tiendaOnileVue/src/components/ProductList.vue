<template>
  <div class="grid">
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
import "../assets/styles/ProductList.css";

const products = reactive<Product[]>([
  { nombre: 'Laptop', precio: 1000, stock: 5, disponible: true, imagen: '/images/laptop.jpg' },
  { nombre: 'Mouse', precio: 50, stock: 0, disponible: false, imagen: '/images/mouse.jpg' },
  { nombre: 'Teclado', precio: 80, stock: 3, disponible: true, imagen: '/images/teclado.jpg' },
  { nombre: 'Monitor', precio: 200, stock: 4, disponible: true, imagen: '/images/monitor.jpg' },
  { nombre: 'Silla Gamer', precio: 300, stock: 2, disponible: true, imagen: '/images/silla.jpg' },
  { nombre: 'Auriculares', precio: 120, stock: 6, disponible: true, imagen: '/images/auriculares.jpg' },
  { nombre: 'Webcam', precio: 90, stock: 8, disponible: true, imagen: '/images/webcam.jpg' },
  { nombre: 'Micrófono', precio: 150, stock: 5, disponible: true, imagen: '/images/microfono.jpg' }
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
