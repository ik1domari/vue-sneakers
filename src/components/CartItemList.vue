<script setup>
import CartItem from '@/components/CartItem.vue'
</script>

<template>
  <div class="flex flex-col flex-1 gap-4 mt-8">
    <CartItem
      title="Кроссовки Asics"
      :price="123"
      image-url="/sneakers/sneakers-1.jpg"
    />
    <CartItem
      title="Кроссовки Nike"
      :price="123"
      image-url="/sneakers/sneakers-2.jpg"
    />
    <CartItem
      title="Кроссовки Adidas"
      :price="123"
      image-url="/sneakers/sneakers-3.jpg"
    />
  </div>
</template>
