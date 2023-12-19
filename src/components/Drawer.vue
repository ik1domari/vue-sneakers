<script setup>
import { computed, inject, provide, ref } from 'vue';
import DrawerHead from '@/components/DrawerHead.vue';
import CartItemList from '@/components/CartItemList.vue';
import InfoBlock from '@/components/InfoBlock.vue';
import axios from 'axios';

const { totalPrice } = defineProps({
  totalPrice: Number,
});

const { cart, closeDrawer } = inject('cart');

const isOrderCreating = ref(false);
const orderId = ref(null);

const createOrder = async () => {
  try {
    isOrderCreating.value = true;
    const { data } = await axios.post(
      `https://57c7e5baa6d902c1.mokky.dev/orders`,
      {
        items: cart.value,
        totalPrice,
      },
    );

    cart.value = [];
    orderId.value = data.id;
    return data;
  } catch (err) {
    console.error(err);
  } finally {
    isOrderCreating.value = false;
  }
};

const tax = computed(() => Math.round(totalPrice * 0.05));

const drawerOpen = inject('drawerOpen');
const buttonDisabled = computed(() =>
  isOrderCreating.value ? true : !totalPrice,
);
</script>

<template>
  <div
    class="fixed top-0 left-0 w-full h-full bg-black/50 z-10"
    @click="drawerOpen ? closeDrawer() : null"
  ></div>
  <div
    class="bg-white flex flex-col w-96 h-full fixed top-0 right-0 z-[11] p-8"
  >
    <DrawerHead />
    <InfoBlock
      v-if="!totalPrice && !orderId"
      title="Корзина пуста"
      description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
      imageUrl="/package-icon.png"
    />
    <InfoBlock
      v-else-if="orderId"
      title="Заказ оформлен!"
      :description="`Ваш заказ #${orderId} скоро приедет к вам.`"
      imageUrl="/order-success-icon.png"
    />
    <div v-else class="flex flex-col justify-between h-full">
      <CartItemList />
      <div class="flex flex-col gap-4 mt-7">
        <div class="flex gap-2 justify-between mt-4">
          <span>Итого</span>
          <div class="flex-1 border-b border-slate-300 border-dashed"></div>
          <b>{{ totalPrice }} ₽</b>
        </div>
        <div class="flex gap-2 justify-between mt-4">
          <span>Налог 5%</span>
          <div class="flex-1 border-b border-slate-300 border-dashed"></div>
          <b>{{ tax }} ₽</b>
        </div>
        <button
          @click="createOrder"
          :disabled="buttonDisabled"
          class="w-full bg-lime-500 text-white rounded-xl py-4 mt-4 transition hover:bg-lime-600 active:bg-lime-700 disabled:bg-slate-400 disabled:cursor-not-allowed"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
