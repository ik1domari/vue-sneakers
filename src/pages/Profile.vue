<script>
import axios from 'axios';
import { ref, onMounted } from 'vue';

export default {
  name: 'Profile',
  setup() {
    const url = 'https://57c7e5baa6d902c1.mokky.dev/orders';
    const orders = ref([]);
    const isLoading = ref(true);
    const error = ref(null);

    const fetchOrders = async () => {
      try {
        const { data } = await axios.get(url);
        orders.value = data;
      } catch (e) {
        console.error('Ошибка при загрузке заказов:', e);
        error.value = 'Не удалось загрузить заказы';
      } finally {
        isLoading.value = false;
      }
    };

    onMounted(fetchOrders);

    return {
      orders,
      isLoading,
      error,
    };
  },
};
</script>

<template>
  <h1 class="text-3xl font-bold">Профиль</h1>

  <div class="mt-4">
    <a
      class="flex items-center gap-2 transition hover:text-green-500"
      href="https://github.com/mikrocosmos"
      target="_blank"
      rel="noreferrer"
    >
      <img
        class="profile__image profile__image--github"
        src="/github.png"
        alt="github"
        :width="30"
        :height="30"
      />
      Мой GitHub
    </a>
  </div>

  <div class="mt-4">
    <h2 class="text-2xl font-bold">Заказы</h2>

    <div v-if="isLoading" class="mt-4">Загрузка заказов...</div>

    <div v-else-if="error" class="mt-4 text-red-500">{{ error }}</div>

    <div v-else-if="orders.length === 0" class="mt-4">
      У вас пока нет заказов
    </div>

    <div v-else class="flex flex-col gap-4 mt-4">
      <div
        v-for="order in orders"
        :key="order.id"
        class="p-4 border rounded-lg"
      >
        <h3 class="text-xl font-medium">Заказ #{{ order.id }}</h3>
        <div class="flex flex-col gap-2 mt-2">
          <div
            v-for="item in order.items"
            :key="item.id"
            class="flex items-center gap-4 p-2 hover:bg-gray-50"
          >
            <img
              class="w-12 h-12 object-cover rounded"
              :src="item.imageUrl"
              :alt="item.title"
            />
            <div>
              <p class="font-medium">{{ item.title }}</p>
              <p v-if="item.price" class="text-sm text-gray-500">
                {{ item.price }} ₽
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
