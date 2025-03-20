<template>
  <div class="card" :style="{ backgroundImage: `url(${product.imagen})` }">
    <div class="text-container">
      <h3>{{ product.nombre }}</h3>
      <p>💰 ${{ product.precio }}</p>
      <p>📦 Stock: {{ product.stock }}</p>
      <p :class="product.disponible ? 'available' : 'not-available'">
        {{ product.disponible ? '✅ Disponible' : '❌ No Disponible' }}
      </p>
    </div>

    <div class="button-group">
      <button @click="modifyStock(1)" class="button increase">➕</button>
      <button @click="modifyStock(-1)" :disabled="product.stock <= 0" class="button decrease">
        ➖
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { Product } from '../types/Product';
import { defineProps, defineEmits } from 'vue';
import "../assets/styles/ProductItem.css";

const props = defineProps<{ product: Product; index: number }>();
const emit = defineEmits<{
  (event: 'update-stock', index: number, amount: number): void;
}>();

const modifyStock = (amount: number) => {
  emit('update-stock', props.index, amount);
};
</script>
