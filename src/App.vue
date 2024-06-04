<script setup>
import { ref, reactive, onMounted } from "vue"
import { db } from "./data/guitars"
import Guitar from "./components/Guitar.vue"
import Header from "./components/Header.vue"
import Footer from "./components/Footer.vue"

const guitars = ref([])
const cart = ref([])
const guitarOffer = ref([])

onMounted(() => {
  guitars.value = db

  const cartStorage = localStorage.getItem("cart")
  //check if exist any element in local storage
  if (cartStorage) {
    cart.value = JSON.parse(cartStorage)
  }

  //set random guitar offer
  const randomGuitar = Math.floor(Math.random() * guitars.value.length + 1)
  guitarOffer.value = db[randomGuitar]
})

// save in localStorage
const saveLocalStorage = () => {
  localStorage.setItem("cart", JSON.stringify(cart.value))
}
const addCart = (guitar) => {
  const existCart = cart.value.findIndex((product) => product.id === guitar.id)

  if (existCart >= 0) {
    cart.value[existCart].amount++
  } else {
    guitar.amount = 1
    cart.value.push(guitar)
  }
  saveLocalStorage()
}
const decreaseAmount = (id) => {
  //search guitar id to decrease amount
  const index = cart.value.findIndex((product) => product.id === id)
  if (cart.value[index].amount <= 1) return
  cart.value[index].amount--
  saveLocalStorage()
}
const increaseAmount = (id) => {
  const index = cart.value.findIndex((product) => product.id === id)
  if (cart.value[index].amount >= 5) return

  cart.value[index].amount++
  saveLocalStorage()
}
const deleteProduct = (id) => {
  // get elements diferent of this id
  cart.value = cart.value.filter((product) => product.id !== id)
  saveLocalStorage()
}
const emptyCart = (id) => {
  // delete all elements
  cart.value = []
  saveLocalStorage()
}
</script>

<template>
  <Header
    v-bind:cart="cart"
    :guitarOffer="guitarOffer"
    @decrease-amount="decreaseAmount"
    @increase-amount="increaseAmount"
    @add-Cart="addCart"
    @delete-product="deleteProduct"
    @empty-cart="emptyCart"
  />

  <main class="container-xl mt-5">
    <h2 class="text-center">Collection</h2>

    <div class="row mt-5">
      <Guitar
        v-for="guitar in guitars"
        v-bind:guitar="guitar"
        @add-Cart="addCart"
      />
    </div>
  </main>

  <Footer />
</template>

<style lang="scss" scoped></style>
