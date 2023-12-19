<script setup>
import { computed, onMounted, provide, reactive, ref, watch } from 'vue';
import axios from 'axios';
import Header from '@/components/Header.vue';
import Drawer from '@/components/Drawer.vue';

const items = ref([]);
const cart = ref([]);

const totalPrice = computed(() =>
  cart.value.reduce((sum, item) => sum + item.price, 0),
);

const drawerOpen = ref(false);

const closeDrawer = () => {
  drawerOpen.value = false;
};

const openDrawer = () => {
  drawerOpen.value = true;
};

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
});

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      `https://57c7e5baa6d902c1.mokky.dev/favorites`,
    );
    items.value = items.value.map(item => {
      const favorite = favorites.find(favorite => favorite.item_id === item.id);

      if (!favorite) {
        return item;
      }

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

const addToCart = item => {
  item.isAdded = true;
  cart.value.push(item);
};

const removeFromCart = item => {
  item.isAdded = false;
  cart.value.splice(cart.value.indexOf(item), 1);
};

const onClickPlus = item => {
  if (!item.isAdded) {
    addToCart(item);
  } else {
    removeFromCart(item);
  }
};

const addToFavorites = async item => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id,
      };
      item.isFavorite = true;

      const { data } = await axios.post(
        `https://57c7e5baa6d902c1.mokky.dev/favorites`,
        obj,
      );

      item.favoriteId = data.id;
    } else {
      item.isFavorite = false;
      await axios.delete(
        `https://57c7e5baa6d902c1.mokky.dev/favorites/${item.favoriteId}`,
      );
      item.favoriteId = null;
    }
  } catch (e) {
    console.log(e);
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
      favoriteId: null,
      isAdded: false,
    }));
  } catch (e) {
    console.log(e);
  }
};

onMounted(async () => {
  const cartItems = localStorage.getItem('cart');
  cart.value = cartItems ? JSON.parse(cartItems) : [];

  await fetchItems();
  await fetchFavorites();

  items.value = items.value.map(item => ({
    ...item,
    isAdded: cart.value.some(cartItem => cartItem.id === item.id),
  }));
});

watch(filters, fetchItems);
watch(cart, () => {
  items.value = items.value.map(item => ({
    ...item,
    isAdded: false,
  }));
});

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value));
  },
  {
    deep: true,
  },
);

provide('drawerOpen', drawerOpen);
provide('cart', {
  cart,
  openDrawer,
  closeDrawer,
  addToCart,
  removeFromCart,
  onClickPlus,
});
provide('items', items);
provide('addToFavorites', addToFavorites);
provide('filters', filters);
</script>

<template>
  <div class="w-4/5 mx-auto mt-10 bg-white rounded-xl shadow-xl">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />
    <Drawer
      :cart-button-disabled="cartButtonDisabled"
      @create-order="createOrder"
      :total-price="totalPrice"
      v-if="drawerOpen"
    />
    <div class="p-8">
      <router-view></router-view>
    </div>
  </div>
</template>
