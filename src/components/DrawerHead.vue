<script setup>
    import { inject } from 'vue'
    const { closeDrawer } = inject('cart')

</script>

<template>
    <div class="flex items-center gap-5 mb-8">
        <h2 class="text-2xl font-bold cursor-default">Корзина</h2>
    </div>
</template>