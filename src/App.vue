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
  kart.value.push(guitar)
  kart.amount = 1
  console.log(kart.value)
}
</script>

<template>
  <Header :kart="kart" />

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
