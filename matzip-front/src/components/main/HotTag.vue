<template>
  <div class="hot-list">
    <div class="header">
      <h2>🔖 HOT Tag</h2>
    </div>
    <ul>
      <li v-for="(tag, index) in tags" :key="index">
        {{ index + 1 }}.&nbsp;&nbsp; {{ tag }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  tags: {
    type: Array,
    default: () => [],
  },
});
</script>

<style scoped>

.hot-list {
  background-color: #fff; /* 회색 배경 */
  border: 1px solid #FF7315;
  border-radius: 8px; /* 모서리 둥글게 */
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1); /* 그림자 효과 */
  min-width: 250px;

}

.header{
  padding: 20px 0 1px 12px;
  background-color: #FF7315;
  color:#fff
}

h2 {
  margin-bottom: 20px; /* 제목과 리스트 사이의 간격 */
  display: flex;
  font-size: 25px;
  font-weight: bold;
  justify-content: left;
}

ul {
  list-style-type: none; /* 기본 리스트 스타일 제거 */
  padding: 0; /* 기본 패딩 제거 */
}

li {
  display: flex; /* 플렉스 박스로 정렬 */
  margin: 15px 0 0 20px;
}
</style>
