<script setup>
import { ref, reactive, onMounted } from "vue"
import { db } from "./data/guitars"
import Guitar from "./components/Guitar.vue"
import Header from "./components/Header.vue"
import Footer from "./components/Footer.vue"
// const state = reactive({
//   guitars: db,
// })
const guitars = ref([])
const cart = ref([])
const guitarOffer = ref([])

onMounted(() => {
  guitars.value = db
  guitarOffer.value = db[3]
})

const addCart = (guitar) => {
  const existCart = cart.value.findIndex((product) => product.id === guitar.id)

  if (existCart >= 0) {
    cart.value[existCart].amount++
    console.log(guitar.amount)
  } else {
    guitar.amount = 1
    cart.value.push(guitar)
  }
}
const decreaseAmount = (id) => {
  //search guitar id to decrease amount
  const index = cart.value.findIndex((product) => product.id === id)
  if (cart.value[index].amount <= 1) return
  cart.value[index].amount--
}
const increaseAmount = (id) => {
  console.log(id)
  const index = cart.value.findIndex((product) => product.id === id)
  if (cart.value[index].amount >= 5) return

  cart.value[index].amount++
}
const deleteProduct = (id) => {
  // get elements diferent of this id
  cart.value = cart.value.filter((product) => product.id !== id)
}
const emptyCart = (id) => {
  // delete all elements
  cart.value = []
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
