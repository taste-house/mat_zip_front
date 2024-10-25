<script setup>
import Header from "@/components/common/header/Header.vue";
import { RouterView } from 'vue-router';
import { useAuthStore } from "@/components/stores/auth.js";

const authStore = useAuthStore();
import { useRoute } from 'vue-router';
import { computed } from 'vue';

const route = useRoute();

// 현재 경로에 '/admin'이 포함되어 있는지 확인
const isAdminRoute = computed(() => route.path.includes('/admin'));

</script>

<template>
  <header v-if="!isAdminRoute">
    <Header/>
  </header>

  <section class="flex-shrink-0">
  <div class="container">
    <RouterView/>
  </div>
</section>

<!--  <RouterView/>-->
</template>

<style scoped>
#app {
  text-align: center;
}
</style>
