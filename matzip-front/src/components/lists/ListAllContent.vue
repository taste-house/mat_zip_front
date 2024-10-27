<script setup>
import { useRouter } from 'vue-router'; // Vue Router를 사용하기 위해 import
defineProps({
  lists: Array
});

const router = useRouter(); // useRouter 훅을 사용하여 router 인스턴스 가져오기

// 리스트 항목 클릭 시 호출될 함수
const navigateToDetail = (listSeq) => {
  router.push(`/user/list/detail/${listSeq}`); // 해당 리스트 상세 페이지로 이동
};
</script>

<template>
  <div class="item-container">
    <div
        v-for="(list, index) in lists"
        :key="index"
        class="list-item"
        @click="navigateToDetail(list.listSeq)"
    >
    <div class="header">
      <h3 class="title">{{ list.listTitle }}</h3>
      <h5 class="nickname">by {{ list.userNickname }}</h5>
    </div>
    <div class="body">
      <p class="content">{{ list.listContent }}</p>
    </div>
    <div class="footer">
      <span class="like">Likes: {{ list.listLike }}</span>
    </div>
  </div>
  </div>
</template>

<style scoped>
.item-container {
  display: flex;
  flex-wrap: wrap; /* 여러 줄로 배치 */
  gap: 50px; /* 아이템 간 간격 */
  justify-content: space-between; /* 공간을 균등하게 분배 */
}

.list-item {
  flex: 1 1 calc(33.33% - 20px); /* 3열 배치 */
  min-width: 240px;
  max-width: 350px;
  min-height: 240px;
  max-height: 350px;
  padding: 10px;
  border: 1px solid #ddd;
  box-sizing: border-box;
  border-radius: 8px;
  background-color: #f9f9f9;
  display: flex;
  flex-direction: column;
  justify-content: space-between; /* footer를 하단에 고정 */
  cursor: pointer; /* 마우스 커서를 포인터로 변경 */
}

.title {
  font-size: 36px;
  font-weight: bold;
  color: #FF7315;
}

.nickname {
  font-size: 20px;
  color: #000000;
  margin: 0 0 20px 0;
}

.body {
  flex-grow: 1;
}

.content {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis; /* 넘칠 경우 ...으로 표시 */
}

.footer {
  text-align: right; /* 아래쪽에 고정된 상태로 오른쪽 정렬 */
}

.like {
  font-size: 16px;
  color: #333;
}
</style>
