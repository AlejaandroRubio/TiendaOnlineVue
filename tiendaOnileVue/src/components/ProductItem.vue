<template>
    <div class="product">
      <h3>{{ product.nombre }}</h3>
      <p>Precio: ${{ product.precio }}</p>
      <p>Stock: {{ product.stock }}</p>
      <p :class="{ available: product.disponible, unavailable: !product.disponible }">
        Disponible: {{ product.disponible ? 'Sí' : 'No' }}
      </p>
      <button @click="modifyStock(1)">Agregar Stock</button>
      <button @click="modifyStock(-1)" :disabled="product.stock <= 0">Reducir Stock</button>
    </div>
  </template>
  
  <script setup lang="ts">
  import type { Product } from '../types/Product';
  import { defineProps, defineEmits } from 'vue';
  
  const props = defineProps<{ product: Product; index: number }>();
  const emit = defineEmits<{
    (event: 'update-stock', index: number, amount: number): void;
  }>();
  
  const modifyStock = (amount: number) => {
    emit('update-stock', props.index, amount);
  };
  </script>
  
  <style scoped>
  .available { color: green; }
  .unavailable { color: red; }
  </style>
  