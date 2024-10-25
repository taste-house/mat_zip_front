<script setup>
import { ref, reactive, onMounted } from "vue";
import ReviewMap from '@/component/review/ReviewMap.vue';
import axios from "axios";

const stateRestaurant = reactive({
  restaurants: [],
  currentPage: 1,
  totalPages: 1,
  totalItems: 0
})

const fetchRestaurant = async () => {
  try {
    const response = await axios.get(`http://localhost:8000/back/api/v1/restaurant`, {
      params: {
        page: 1,
        size: 100
      }
    });
    stateRestaurant.restaurants = response.data.restaurants;
    stateRestaurant.currentPage = response.data.currentPage;
    stateRestaurant.totalPages = response.data.totalItems;
    stateRestaurant.totalItems = response.data.totalItems;
  } catch (error) {
    console.error(`음식점 목록 불러오기 실패!`, error);
  }
}

onMounted(() => {
  fetchRestaurant();
  });
</script>

<template>
  <h1>Review - View</h1>
  <div>
    <ReviewMap :restaurants="stateRestaurant.restaurants"/>
  </div>
</template>

<style scoped></style>
