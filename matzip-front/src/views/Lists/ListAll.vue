<script setup>
import ListAllContent from "@/components/lists/ListAllContent.vue";
import  { ref, onMounted } from "vue";
import axios from 'axios';

const lists = ref([]) // 백에서 넘어노는 데이터 저장하는 배열
const errorMessage = ref(null);
const page = ref(1);
const size = ref(5);

//데이터 불러오기 ex
const fetchData = async () => {
  try {
    const response = await axios.get('http://localhost:8000/back/api/v1/listbox', {
      params: {
        page: page.value,
        size: size.value,
      }
    });

    if(response.data && response.data.data2 && response.data.data2.listSearchAllDTOs){
      lists.value = response.data.data2.listSearchAllDTOs;
    }else{
      lists.value = []; // 데이터 없을때 빈 배열 호출
      errorMessage.value='리스트가 없습니다.'
    }
  } catch(error){
    console.error('데이터 로딩 중 오류 발생:', error);
    errorMessage.value = error.response ? error.response.data.message : error.message; // 에러메세지 저장
  }
};

onMounted(fetchData);
</script>

<template>
  <div class="Container">
    <div class="header">
      <img src="../../asserts/ListText.png" alt="리스트 텍스트" class="listText">
    </div>
    <div class="listContent">
      <list-all-content v-if="!errorMessage" :lists="lists"/>
      <p v-else>{{ errorMessage }}</p> <!-- 에러 메시지 출력 -->
    </div>
  </div>
</template>

<style scoped>
.listText{
  margin: 20px 0 0 0;
  width: 60px;
}
</style>