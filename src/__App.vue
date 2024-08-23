<script setup>
import { ref, watch, provide, computed } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'


// КОРЗИНА (START)
const cart = ref([])
const isCreatingOrder = ref(false)

const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

const vatPrice = computed(() => Math.round(totalPrice.value * 0.05))

const isCartEmpty = computed(() => cart.value.length === 0)

const cartButtonDisabled = computed(() => isCreatingOrder.value || isCartEmpty.value)

const closeDrawer = () => {
    drawerOpen.value = false
}

const openDrawer = () => {
    drawerOpen.value = true
}

const addToCart = (item) => {
    cart.value.push(item)
    item.isAdded = true
}

const removeFromCart = (item) => {
    cart.value.splice(cart.value.indexOf(item), 1)
    item.isAdded = false
}

const createOrder = async () => {
    try {
        isCreatingOrder.value = true
        const { data } = await axios.post(`https://e0665820576ea883.mokky.dev/orders`, {
            items: cart.value,
            totalPrice: totalPrice.value,
        })
        cart.value = []
        return data
    } catch (err) {
        console.log(err)
    } finally {
        isCreatingOrder.value = false
    }
}

watch(cart, () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
},
    { deep: true }
)

provide('cart', {
    cart,
    closeDrawer,
    openDrawer,
    addToCart,
    removeFromCart,
})
// КОРЗИНА (END)


</script>


<template>

    <div>
        <Drawer :total-price="totalPrice" :vat-price="vatPrice" @create-order="createOrder"
            :button-disabled="cartButtonDisabled" v-if="drawerOpen" />

        <div class="bg-white w-4/5 mx-auto rounded-3xl shadow-xl mt-14 mb-6 ">
            <Header :total-price="totalPrice" @openDrawer="openDrawer" />
        </div>

        <div class="bg-white w-4/5 mx-auto rounded-3xl shadow-2xl mb-14">

            <div class="p-10 pb-12">
                <!-- <Home /> -->
                <router-view></router-view>
            </div>

        </div>


    </div>



</template>
