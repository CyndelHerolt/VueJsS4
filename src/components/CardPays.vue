<script setup>
defineProps(['pays', 'capitale', 'image', 'datapays'])
</script>

<template>
  <ul>
    <li>Pays : {{ pays }}</li>
    <!--    <li>Capitale : {{ capitale }}</li>-->
    <li><img :src="image" alt="drapeau" style="width: 100px"></li>
    <li>Data : {{ datapays }}</li>
    <routerLink :to="'/fichepays/'+pays">complément d'info</routerLink>
  </ul>
</template>
