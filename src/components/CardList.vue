<script setup>
import Card from '@/components/Card.vue';
import { inject } from 'vue';

defineProps({
  items: Array,
  isFavorite: Boolean,
});

const emit = defineEmits(['addToFavorites', 'onClickPlus']);
</script>

<template>
  <div class="grid grid-cols-4 gap-4 mt-8" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :image-url="item.imageUrl"
      :price="item.price"
      :is-favorite="item.isFavorite"
      :on-click-favorite="
        isFavorite ? null : () => emit('addToFavorites', item)
      "
      :on-click-add="isFavorite ? null : () => emit('onClickPlus', item)"
      :is-added="item.isAdded"
    />
  </div>
</template>
