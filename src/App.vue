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
const kart = ref([])

onMounted(() => {
  guitars.value = db
})

const addKart = (guitar) => {
  const existKart = kart.value.findIndex((product) => product.id === guitar.id)

  if (existKart >= 0) {
    kart.value[existKart].amount++
    console.log(guitar.amount)
  } else {
    guitar.amount = 1
    kart.value.push(guitar)
  }
}
const decreaseAmount = (id) => {
  const index = kart.value.findIndex((product) => product.id === id)
  if (kart.value[index].amount <= 1) return
  kart.value[index].amount--
  console.log(id)
  // guitar.amount--
}
const increaseAmount = (id) => {
  console.log(id)
  const index = kart.value.findIndex((product) => product.id === id)
  kart.value[index].amount++
  // guitar.amount++
}
</script>

<template>
  <Header
    v-bind:kart="kart"
    @decrease-amount="decreaseAmount"
    @increase-amount="increaseAmount"
  />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitar
        v-for="guitar in guitars"
        v-bind:guitar="guitar"
        @add-Kart="addKart"
      />
    </div>
  </main>

  <Footer />
</template>

<style lang="scss" scoped></style>
