<script setup>

    defineProps({
        totalPrice: Number,
    })

    const emit = defineEmits(['openDrawer'])

</script>

<template>
    <header class="flex items-center justify-between border-b border-slate-200 px-10 py-8">
        <router-link to="/">
            <div class="flex items-center gap-4">
                <img src="/logo.png" alt="Logo" class="w-10" />
                <div class="cursor-default">
                    <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
                    <p class="text-slate-400">Магазин лучших кроссовок</p>
                </div>
            </div>
        </router-link>

        <ul class="flex items-center gap-10">
            <li @click="() => emit('openDrawer')" class="flex items-center cursor-pointer gap-3 text-slate-400 hover:text-black ease-linear duration-150">
                <img src="/cart.svg" alt="Cart" />
                <span>{{ totalPrice }} руб.</span>
            </li>
            <router-link to="/favorites">
                <li class="flex items-center cursor-pointer gap-3 text-slate-400 hover:text-black ease-linear duration-150">
                    <img src="/heart.svg" alt="Cart" />
                    <span>Закладки</span>
                </li>
            </router-link>
            <li class="flex items-center cursor-pointer gap-3 text-slate-400 hover:text-black ease-linear duration-150">
                <img src="/profile.svg" alt="Cart" />
                <span>Профиль</span>
            </li>
        </ul>
    </header>
</template>
