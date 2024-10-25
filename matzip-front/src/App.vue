<script setup>
import Header from "@/components/common/header/Header.vue";
import { RouterView } from 'vue-router';

</script>

<template>
<!--<header>-->
<!--  <Header/>-->
<!--</header>-->
<!--<section class="flex-shrink-0">-->
<!--  <div class="container">-->
<!--    <RouterView/>-->
<!--  </div>-->
<!--</section>-->

  <RouterView/>
</template>

<style scoped>
#app {
  text-align: center;
}
</style>
