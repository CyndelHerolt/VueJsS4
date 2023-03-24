<script setup>
const username = 'Enguerrand'
</script>

<template>
  <h1>MMI world</h1>

  <nav>
    <p>
      Ici mettre le menu des routes vers mes pages
    </p>
    <ul>
      <li>
        <RouterLink to="/">Accueil</RouterLink>
      </li>
      <li>
        <RouterLink to="/about">A propos</RouterLink>
      </li>
      <li>
        <RouterLink to="/pays">Pays</RouterLink>
      </li>
      <li>
        <RouterLink to="/recherche">Recherche</RouterLink>
      </li>
      <li>
        <RouterLink v-bind:to="`/user/${username}`">User</RouterLink>
        <!--    version courte    -->
        <!--        <router-link :to="`/user/${username}`">User</router-link>-->
      </li>
    </ul>
  </nav>

  <p>Ici mettre le routeur view pour afficher chaque page</p>

  <router-view>

  </router-view>
</template>

<style scoped></style>
