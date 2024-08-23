
<script setup>
    
    import Card from './Card.vue'

    defineProps({
        items: Array,
        isFavorites: Boolean,
    })

    const emit = defineEmits(['addToFavorite', 'addToCart'])

</script>

<template>

    <div v-auto-animate id="cards" class="grid grid-cols-4 gap-10">
        <Card
            v-for="item in items"
            :key="item.id"
            :id="item.id"
            :image-url="item.imageUrl"
            :title="item.title"
            :price="item.price"
            :is-added="item.isAdded"
            :is-favorite="item.isFavorite"
            :on-click-favorite="isFavorites ? null : () => emit('addToFavorite', item)"
            :on-click-add="isFavorites ? null : () => emit('addToCart', item)"
            
        />
    </div>

</template>
