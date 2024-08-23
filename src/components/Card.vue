<script setup>

    const props = defineProps({
        id: Number,
        imageUrl: String,
        price: Number,
        title: String,
        isFavorite: Boolean,
        isAdded: Boolean,
        onClickAdd: Function,
        onClickFavorite: Function,
    })

</script>

<template>

    <div id="card" class="relative flex flex-col justify-between bg-white p-8 border border-slate-100 hover:border-slate-200 rounded-[40px] cursor-pointer hover:shadow-xl hover:-translate-y-2 ease-out duration-300">

        <img v-if="onClickFavorite" @click="onClickFavorite" :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'" alt="Like 1" class="absolute top-8 left-8">
        <!-- <img src="/sneakers/sneakers-1.jpg" alt="Sneaker"> -->
        <img :src="imageUrl" alt="Sneaker">

        <div class="">
            <p class="mt-2">{{ title }}</p>

            <div class="flex flex-1 items-center justify-between mt-5">
                <div class="flex flex-col">
                    <span class="text-slate-400">Цена:</span>
                    <b class="flex-1">{{ price }} руб.</b>
                </div>
                <img v-if="onClickAdd" @click="onClickAdd" :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="Plus">
            </div>
        </div>

    </div>

</template>