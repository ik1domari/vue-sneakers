<script setup>
import { inject, onMounted, ref } from 'vue';
import axios from 'axios';
import CardList from '@/components/CardList.vue';

const favorites = ref([]);

const addToFavorites = inject('addToFavorites');

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://57c7e5baa6d902c1.mokky.dev/favorites?_relations=items',
    );

    favorites.value = data.map(obj => obj.item);
  } catch (e) {
    console.log(e);
  }
});
</script>

<template>
  <h1>Закладки</h1>
  <CardList :items="favorites" is-favorite @add-to-favorites="addToFavorites" />
</template>
