<script setup>
import Card from '@/components/Card.vue';

defineProps({
  items: Array,
});

const emit = defineEmits(['addToFavorites']);
</script>

<template>
  <div class="grid grid-cols-4 gap-4 mt-8">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :image-url="item.imageUrl"
      :price="item.price"
      :is-favorite="item.isFavorite"
      :on-click-favorite="() => emit('addToFavorites', item)"
    />
  </div>
</template>
