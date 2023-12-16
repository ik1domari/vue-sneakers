<script setup>
const emit = defineEmits(['openDrawer']);

defineProps({
  totalPrice: Number,
});
</script>

<template>
  <header
    class="flex items-center justify-between py-5 px-8 border-b border-slate-100"
  >
    <div class="flex items-center gap-4">
      <img src="/logo.png" alt="logo" class="w-10" />
      <div>
        <h2 class="font-bold uppercase text-xl">Vue Sneakers</h2>
        <p class="text-slate-400">Магазин лучших кроссовок</p>
      </div>
    </div>

    <ul class="flex gap-10 items-center text-gray-500">
      <li
        @click="emit('openDrawer')"
        class="flex gap-3 items-center transition hover:text-black cursor-pointer"
      >
        <img src="/cart.svg" alt="cart" />
        <span class="font-semibold">{{ totalPrice }} ₽</span>
      </li>

      <li
        class="flex gap-3 items-center transition hover:text-black cursor-pointer"
      >
        <img src="/heart.svg" alt="favorite" />
        <span>Закладки</span>
      </li>
      <li
        class="flex gap-3 items-center transition hover:text-black cursor-pointer"
      >
        <img src="/profile.svg" alt="profile" />
        <span>Профиль</span>
      </li>
    </ul>
  </header>
</template>
