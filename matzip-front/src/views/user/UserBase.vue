<script setup>
import { RouterView } from 'vue-router';
import Header from "@/components/common/header/Header.vue";

</script>

<template>
  <header>
    <Header/>
  </header>
  <section>
    <RouterView/>
  </section>
</template>

<style scoped>

</style>