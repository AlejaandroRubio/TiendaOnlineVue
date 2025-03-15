<template>
  <div class="border-4 border-black rounded-lg shadow-lg overflow-hidden w-300px h-200px"
  :style="{
    width: '300px',
    height: '200px'
  }">
    <div 
      class="relative flex flex-col justify-between items-center w-full h-full"
      :style="{
        backgroundImage: `url(${product.imagen})`,
        backgroundSize: 'cover',
        backgroundPosition: 'center',
        backgroundRepeat: 'no-repeat'
      }"
    >
      <div class="absolute inset-0 bg-black bg-opacity-40"></div>

      <div class="relative z-10 text-white p-4 text-center">
        <h3 class="text-xl font-semibold">{{ product.nombre }}</h3>
        <p class="text-lg">💰 ${{ product.precio }}</p>
        <p class="text-lg">📦 Stock: {{ product.stock }}</p>
        <p :class="product.disponible ? 'text-green-300' : 'text-red-400'">
          {{ product.disponible ? '✅ Disponible' : '❌ No Disponible' }}
        </p>
      </div>

      <div class="relative z-10 flex justify-center gap-2 p-2">
        <button @click="modifyStock(1)" class="px-3 py-1 bg-blue-500 text-white rounded-lg hover:bg-blue-700">
          ➕
        </button>
        <button 
          @click="modifyStock(-1)" 
          :disabled="product.stock <= 0"
          class="px-3 py-1 bg-red-500 text-white rounded-lg hover:bg-red-700 disabled:bg-gray-400"
        >
          ➖
        </button>
      </div>
    </div>
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
