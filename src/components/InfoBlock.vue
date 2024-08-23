<script setup>

    defineProps({
        title: String,
        description: String,
        imageUrl: String,
    })

</script>

<template>
    <div>
        <div class="text-center flex flex-col items-center">
            <img height="80" width="80" :src="imageUrl" alt="Info Image">
            <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
            <p class="mt-2 text-gray-400">{{ description }}</p>
        </div>
    </div>
</template>



