<script setup>
    const emit = defineEmits(['onClickRemove'])

    defineProps({
        id: Number,
        title: String,
        imageUrl: String,
        price: Number
    })
</script>

<template>
    <div class="cursor-default">
        <div class="flex items-center gap-4 w-full border border-slate-100 p-4 rounded-xl hover:shadow-xl hover:-translate-y-2 hover:border-slate-200 transition duration-500">
            <img class="w-16 h-16" :src="imageUrl" alt="Sneaker">
            <div class="flex flex-col">
                <p>{{ title }}</p>
                <div class="flex items-end justify-between mt-2">
                    <b>{{ price }} руб.</b>
                    <img @click="emit('onClickRemove')" class="opacity-60 hover:opacity-100 transition-all" src="/close.svg" alt="Close">
                </div>
            </div>
        </div>
    </div>
</template>



