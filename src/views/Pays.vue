<script setup>
// const countries = Array.from({ length: 10 }, (_, i) => i + 1)
// const indice = Array(10)
import CardPays from "../components/CardPays.vue";

import {onMounted, ref} from 'vue'
import axios from 'axios'

const data = ref('')
const recherche = ref('')
const dataAll = ref('')

onMounted(async () => {
  const response = await axios.get('https://restcountries.com/v3.1/all')
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

    <label for="recherche">Rechercher un pays</label>
    <input v-model="recherche" @keyup="filter" type="text">
    <!--    <p>Message is: {{ recherche }}</p>-->
    <button @click="filter">Filtrer</button>

    <ul v-for="pays in data">
      <card-pays :pays="pays.name.common" :image="pays.flags.svg" :datapays="pays"/>
    </ul>

    <h2>Liste de 10 pays</h2>
    <ul>
      <li v-for=" indice in 10">
        pays numéro {{ indice }}
        <RouterLink v-bind:to="'/fichepays/'+indice">complément d'info</RouterLink>
      </li>
    </ul>
    <p>Contenu de la page fiche pays, c'est super !</p>
  </main>
</template>
