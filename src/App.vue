<script setup>
import { onMounted, provide, reactive, ref, watch } from 'vue';
import axios from 'axios';

import Header from '@/components/Header.vue';
import CardList from '@/components/CardList.vue';
import Drawer from '@/components/Drawer.vue';

const items = ref([]);

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
});

const onChangeSelect = event => {
  filters.sortBy = event.target.value;
};

const onChangeSearchInput = event => {
  filters.searchQuery = event.target.value;
};

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      `https://57c7e5baa6d902c1.mokky.dev/favorites`,
    );
    items.value = items.value.map(item => {
      const favorite = favorites.find(favorite => favorite.itemId === item.id);

      if (!favorite) {
        return item;
      }
      console.log(items.value);
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      };
    });
  } catch (e) {
    console.log(e);
  }
};

const addToFavorites = async item => {
  item.isFavorite = !item.isFavorite;
  const favorite = await axios.get(
    `https://57c7e5baa6d902c1.mokky.dev/favorites/${item.id}`,
  );

  if (!favorite) {
    await axios.post(`https://57c7e5baa6d902c1.mokky.dev/favorites`, {
      itemId: item.id,
    });
  } else {
    await axios.delete(
      `https://57c7e5baa6d902c1.mokky.dev/favorites/${item.id}`,
    );
  }
};

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    };

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`;
    }

    const { data } = await axios.get(
      `https://57c7e5baa6d902c1.mokky.dev/items`,
      { params },
    );

    items.value = data.map(item => ({
      ...item,
      isFavorite: false,
      isAdded: false,
    }));
  } catch (e) {
    console.log(e);
  }
};

onMounted(async () => {
  await fetchItems();
  await fetchFavorites();
});
watch(filters, fetchItems);

provide('addToFavorites', addToFavorites);
</script>

<template>
  <div class="w-4/5 mx-auto mt-10 bg-white rounded-xl shadow-xl">
    <Header />
    <!--    <Drawer />-->
    <div class="p-8">
      <div class="flex justify-between items-center">
        <h2 class="font-bold text-3xl">Все кроссовки</h2>
        <div class="flex items-center gap-4">
          <select
            @change="onChangeSelect"
            class="py-2 px-3 border rounded-md outline-none"
          >
            <option value="name">По названию</option>
            <option value="price">Сначала дешевые</option>
            <option value="-price">Сначала дорогие</option>
          </select>
          <div
            class="flex items-center rounded-md gap-4 border border-slate-300 px-4 py-2"
          >
            <img src="/search.svg" alt="search" />

            <input
              @input="onChangeSearchInput"
              class="outline-none focus:border-gray-400"
              placeholder="Поиск"
            />
          </div>
        </div>
      </div>

      <div class="mt-10">
        <CardList :items="items" @addToFavorites="addToFavorites" />
      </div>
    </div>
  </div>
</template>
