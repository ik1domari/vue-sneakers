<script setup>
import { inject } from 'vue';
import DrawerHead from '@/components/DrawerHead.vue';
import CartItemList from '@/components/CartItemList.vue';

const drawerOpen = inject('drawerOpen');
const closeDrawer = inject('cartActions').closeDrawer;
</script>

<template>
  <div
    class="fixed top-0 left-0 w-full h-full bg-black/50 z-10"
    @click="drawerOpen ? closeDrawer() : null"
  ></div>
  <div
    class="bg-white flex flex-col justify-between w-96 h-full fixed top-0 right-0 z-[11] p-8"
  >
    <DrawerHead />
    <CartItemList />
    <div class="flex flex-col gap-4 mt-7">
      <div class="flex gap-2 justify-between mt-4">
        <span>Итого</span>
        <div class="flex-1 border-b border-slate-300 border-dashed"></div>
        <b>$123</b>
      </div>
      <div class="flex gap-2 justify-between mt-4">
        <span>Налог 5%</span>
        <div class="flex-1 border-b border-slate-300 border-dashed"></div>
        <b>$12</b>
      </div>
      <button
        disabled
        class="w-full bg-lime-500 text-white rounded-xl py-4 mt-4 transition hover:bg-lime-600 active:bg-lime-700 disabled:bg-slate-400 disabled:cursor-not-allowed"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
