<script setup>
const emit = defineEmits(['onClickRemove']);

defineProps({
  imageUrl: String,
  title: String,
  price: Number,
});
</script>

<template>
  <div class="flex gap-4 items-center border border-slate-200 p-4 rounded-xl">
    <img class="w-16 h-16" :src="imageUrl" :alt="title" />
    <div class="flex flex-col">
      <p>{{ title }}</p>
      <div class="flex justify-between mt-2">
        <b>{{ price }} ₽</b>
        <img
          @click="emit('onClickRemove')"
          class="opacity-40 cursor-pointer transition hover:opacity-100"
          src="/close.svg"
          alt="remove"
        />
      </div>
    </div>
  </div>
</template>
