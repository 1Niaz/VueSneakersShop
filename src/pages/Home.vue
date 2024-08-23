<script setup>

import axios from 'axios'
import CardList from '@/components/CardList.vue';
import { inject, reactive, watch, ref, onMounted } from 'vue';

const { cart, addToCart, removeFromCart } = inject('cart')

const items = ref([])

const filters = reactive({
    sortBy: 'title',
    searchQuery: '',
})

const onClickAddPlus = (item) => {
    if (!item.isAdded) {
        addToCart(item)
    } else {
        removeFromCart(item)
    }
}

const onChangeSelect = event => {
    filters.sortBy = event.target.value
}

const onChangeSearchInput = event => {
    filters.searchQuery = event.target.value
}

const addToFavorite = async (item) => {
    try {
        if (!item.isFavorite) {
            // const obj = {
            //     parentId: item.id,
            // }
            const obj = {
                parentId: item.id,
                item
            }
            item.isFavorite = true
            const { data } = await axios.post(`https://e0665820576ea883.mokky.dev/favorites`, obj)
            item.favoriteId = data.id
        } else {
            item.isFavorite = false
            await axios.delete(`https://e0665820576ea883.mokky.dev/favorites/${item.favoriteId}`)
            item.favoriteId = null
        }
    } catch (err) {
        console.log(err)
    }
}

const fetchFavorites = async () => {
    try {
        const { data: favorites } = await axios.get(`https://e0665820576ea883.mokky.dev/favorites`)
        items.value = items.value.map((item) => {
            const favorite = favorites.find((favorite) => favorite.parentId === item.id)
            if (!favorite) {
                return item
            }
            return {
                ...item,
                isFavorite: true,
                favoriteId: favorite.id
            }
        })
        console.log(items.value)
    } catch (err) {
        console.log(err)
    }
}

const fetchItems = async () => {
    try {
        const params = {
            sortBy: filters.sortBy,
        }

        if (filters.searchQuery) {
            params.title = `*${filters.searchQuery}*`
        }

        const { data } = await axios.get(`https://e0665820576ea883.mokky.dev/items`, {
            params
        })
        items.value = data.map((obj) => ({
            ...obj,
            isFavorite: false,
            favoriteId: null,
            isAdded: false,
        }))
    } catch (err) {
        console.log(err)
    }
}


onMounted(async () => {
    const localCart = localStorage.getItem('cart')
    cart.value = localCart ? JSON.parse(localCart) : []

    await fetchItems()
    await fetchFavorites()

    items.value = items.value.map((item) => ({
        ...item,
        isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
    }))

})

watch(filters, fetchItems)

watch(cart, () => {
    items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
    }))
})


</script>

<template>
    <div>
        <div class="flex items-center justify-between">
            <h2 class="text-3xl font-bold mb-10">Все кроссовки</h2>
            <div class="flex gap-4">
                <select @change="onChangeSelect" name="" id="" class="py-2 px-3 border rounded-xl outline-none">
                    <option value="name">По названию</option>
                    <option value="price">По цене (Дешевые)</option>
                    <option value="-price">По цене (Дорогие)</option>
                </select>
                <div class="relative">
                    <img class="absolute top-3 left-4" src="/search.svg" alt="">
                    <input @input="onChangeSearchInput"
                        class="border rounded-xl py-2 pl-11 pr-4 outline-none focus:border-gray-400"
                        placeholder="Поиск...">
                </div>
            </div>
        </div>
        <div class="mt-8">
            <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
        </div>
    </div>
</template>
