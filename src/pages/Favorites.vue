<script setup>

import { onMounted, ref } from 'vue';
import axios from 'axios'

import CardList from '@/components/CardList.vue';

const favorites = ref([])

onMounted(async () => {
    try {
        const { data } = await axios.get(`https://e0665820576ea883.mokky.dev/favorites`)

        favorites.value = data.map((obj) => obj.item)
        // console.log(data)
    } catch (err) {
        console.log(err)
    }
})

</script>

<template>
    <div>
        <h2 class="text-3xl font-bold mb-10">Мои закладки</h2>

        <CardList :items="favorites" is-favorites />
    </div>
</template>

