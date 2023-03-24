<script setup>

import {onMounted, ref} from 'vue'
import axios from 'axios'

const data = ref('')
const recherche = ref('')
const dataAll = ref('')

onMounted(async () => {
  const response = await axios.get('https://restcountries.com/v3.1/name/')
  data.value = response.data
  dataAll.value = data.value;
})
function filter() {
  data.value = dataAll.value.filter(pays =>
      {
        return pays.name.common.toLowerCase().startsWith(recherche.value.toLowerCase())
      }
  )
}
</script>

<template>
  <main>
    <h1>Recherche</h1>

    <label for="recherche">Rechercher un pays</label>
    <input v-model="recherche" @keyup="filter" type="text">
    <!--    <p>Message is: {{ recherche }}</p>-->
    <button @click="filter">Filtrer</button>
  </main>
</template>
