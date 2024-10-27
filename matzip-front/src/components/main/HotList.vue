<template>
  <div class="hot-list-container">
    <h2 class="hot-list-title">️ 인기 리스트</h2>
    <div class="hot-list-items">
      <div class="hot-list-item"
           v-for="(item, index) in items"
           :key="index"
           :style="{ backgroundColor: getRandomPastelColor() }"
           @click="goToListPage(item.listSeq)">
        <div class="item-name">{{ item.listTitle }}</div>
        <p>{{ item.userName }}</p>
        <p>{{ item.listContent }}</p>
        <p>좋아요: {{ item.listLike }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from 'vue';
import { useRouter } from 'vue-router';

// 인기리스트 데이터 받기
const props = defineProps({
  items: {
    type: Array,
    default: () => [] // 기본값을 빈 배열로 설정
  }
});

const router = useRouter();

// 리스트 페이지로 이동하기
const goToListPage = (listSeq) => {
  router.push(`/user/list/detail/${listSeq}`); // listSeq 값을 URL에 포함해서 이동
};

const getRandomPastelColor = () => {
  const hue = Math.floor(Math.random() * 360);
  const saturation = 70 + Math.random() * 20; // 70-90% 정도의 채도
  const lightness = 85 + Math.random() * 10; // 85-95% 정도의 밝기
  return `hsl(${hue}, ${saturation}%, ${lightness}%)`;
};
</script>

<style scoped>
.hot-list-container {
  margin-top: 50px;
  padding: 20px;
  border-radius: 8px;
  width: 100%;
}

.hot-list-title {
  margin: 20px 0 20px 0;
  font-size: 48px;
  font-weight: bold;
  text-align: left;
}

.hot-list-items {
  display: flex;
  justify-content: space-between;
  gap: 20px;
}

.hot-list-item {
  flex-basis: 18%;
  flex-grow: 1;
  min-width: 240px;
  min-height: 280px;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 10px;
  text-align: left;
}

.item-name {
  font-weight: bold;
}

.hot-list-items{
  display: flex;
  width: 100%;
  border: 1px solid #ccc; /* 회색 테두리 추가 */
  border-radius: 13px; /* 테두리 곡률 설정 */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* 그림자 추가 */
  padding: 16px; /* 내부 여백 추가 */
  background-color: #fff; /* 배경색 추가 */
}
</style>
