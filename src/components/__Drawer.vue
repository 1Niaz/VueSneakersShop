<script setup>
    import { inject } from 'vue'
    import DrawerHead from './DrawerHead.vue';
    import CartItemList from './CartItemList.vue';
    import InfoBlock from './InfoBlock.vue'

    const emit = defineEmits(['createOrder'])

    defineProps({
        totalPrice: Number,
        vatPrice: Number,
        buttonDisabled: Boolean,
    })

    const { closeDrawer } = inject('cart')


</script>

<template>
    <div>
        <div @click="closeDrawer" class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
        <div class="bg-white h-full w-96 fixed right-0 top-0 z-20 px-10 py-8">

            <DrawerHead />

            <div v-if="!totalPrice" class="h-full flex items-center justify-center">
                <InfoBlock title="Корзина пуста" description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ." image-url="/package-icon.png" />
            </div>

            <div v-else>
                <CartItemList />

                <div class="flex flex-col gap-4 my-6">
                    <div class="flex gap-2">
                        <span>Итого:</span>
                        <div class="flex-1 border-b border-dashed"></div>
                        <b>{{ totalPrice }} руб.</b>
                    </div>
                    <div class="flex gap-2">
                        <span>Налог 5%:</span>
                        <div class="flex-1 border-b border-dashed"></div>
                        <b>{{ vatPrice }} руб.</b>
                    </div>
                    <button @click="() => emit('createOrder')" :disabled="buttonDisabled" class="disabled:bg-slate-400 rounded-xl text-white bg-lime-500 hover:bg-lime-600 active:bg-lime-700 transition-all w-full py-3 mt-2 cursor-pointer">
                    <!-- <button class="disabled:bg-slate-400 rounded-xl text-white bg-lime-500 hover:bg-lime-600 active:bg-lime-700 transition-all w-full py-3 mt-2 cursor-pointer"> -->
                        Оформить заказ
                    </button>
                </div>
            </div>

            
        </div>
    </div>
</template>