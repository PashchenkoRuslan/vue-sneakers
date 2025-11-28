<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

import CardList from '../components/CardList.vue'

const favorites = ref([])
const isLoaded = ref(false)

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://d2e60887ee64511b.mokky.dev/favorites?_relations=items'
    )

    favorites.value = data.map((obj) => obj.item)
  } catch (err) {
    console.log(err)
  } finally {
    isLoaded.value = true
  }
})
</script>

<template>
  <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>

  <div
    v-if="!favorites.length && isLoaded"
    class="flex flex-col items-center justify-center mt-24 text-gray-400"
  >
    <div
      class="w-20 h-20 flex items-center justify-center bg-gray-100 rounded-2xl shadow-sm overflow-hidden"
    >
      <img src="/package-icon.png" alt="" class="w-16 h-16 object-contain opacity-80" />
    </div>

    <p class="mt-6 text-2xl font-semibold text-gray-500">Тут пусто</p>

    <p class="mt-2 text-base text-gray-400">
      Добавьте товары в закладки, чтобы вернуться к ним позже
    </p>
  </div>

  <CardList :items="favorites" is-favorites />
</template>
