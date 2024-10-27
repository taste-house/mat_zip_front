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
    const response = await axios.get('https://matzipapi.huichan.kr/back/api/v1/listbox', {
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
      <img src="../../assets/ListText.png" alt="리스트 텍스트" class="listText" />
    </div>
    <div class="listContent">
      <div v-if="!errorMessage" class="listItems">
        <list-all-content :lists="lists" />
      </div>
      <p v-else>{{ errorMessage }}</p> <!-- 에러 메시지 출력 -->
    </div>
  </div>
</template>

<style scoped>
.listText {
  margin: 50px 0 20px 0;
  width: 160px;
}

.listContent {
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 13px;
  background-color: #fff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
}

.listItems {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
  width: 100%;
}


</style>