<script setup>
import CardList from '@/components/CardList.vue';
import { inject } from 'vue';

const items = inject('items');

const filters = inject('filters');

const onChangeSelect = event => {
  filters.sortBy = event.target.value;
};

const onChangeSearchInput = event => {
  filters.searchQuery = event.target.value;
};

const addToFavorites = inject('addToFavorites');
const { onClickPlus } = inject('cart');
</script>

<template>
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
    <CardList
      :items="items"
      @add-to-favorites="addToFavorites"
      @on-click-plus="onClickPlus"
    />
  </div>
</template>
