<template>
  <div class="header">
    <a> 게시판</a>
  </div>
  <div class="home-post-container">
    <div class="board-list">
      <div class="board-item" v-for="(board, index) in boards" :key="index">
        <!-- 게시판 이름 클릭 시 이동 -->
        <h3 class="board-name" @click="onBoardClick(board.seq)">
          📙 {{ board.name }}
        </h3>

        <!-- 주황색 줄 추가 -->
        <div class="divider"></div>

        <!-- 게시판의 게시물 목록 -->
        <ul class="recent-posts">
          <li v-for="(post, postIndex) in board.recentPosts"
              :key="postIndex"
              class="post-item"
              @click="onPostClick(post)">
            - {{ post.title }} by {{ post.userNickname }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup>
import {defineProps} from 'vue';
import {useRouter} from 'vue-router';

const router = useRouter();

// props로 데이터 받기
const props = defineProps({
  boards: {
    type: Array,
    required: true
  }
});

// 게시판 이름 클릭 이벤트 핸들러
const onBoardClick = (boardSeq) => {
  const page = 1; // 페이지 번호
  const size = 5; // 한 페이지당 항목 수
  // 쿼리 파라미터를 사용하여 게시판으로 이동
  router.push({
    path: '/user/post', // 사용자 게시물 페이지 경로
    query: {boardSeq, page, size} // 쿼리 파라미터 설정
  });
};

// 게시물 클릭 이벤트 핸들러
const onPostClick = (post) => {
  // 게시물 ID만 쿼리 파라미터로 전달
  router.push({
    path: `/user/post/${post.seq}`,
  });
};
</script>

<style scoped>

.header{
  font-size:48px;
  font-weight: bold;
  margin:60px 0 20px 0;
}
.home-post-container {
  display: flex;
  width: 100%;
  border: 1px solid #ccc; /* 회색 테두리 추가 */
  border-radius: 13px; /* 테두리 곡률 설정 */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* 그림자 추가 */
  padding: 16px; /* 내부 여백 추가 */
  background-color: #fff; /* 배경색 추가 */
}

.board-list {
  display: flex;
  margin: 20px 0 20px 0;
  justify-content: center;
  flex-wrap: wrap; /* 줄바꿈 허용 */
  gap: 70px; /* 각 게시판 사이의 간격을 130px로 설정 */
}

.board-item {
  flex: 1 1 calc(50% - 65px); /* 두 개씩 한 줄에 배치되도록 설정 (130px 간격을 고려) */
  max-width: calc(50% - 65px); /* 최대 너비 설정 (130px 간격을 고려) */
  padding: 30px; /* 패딩을 늘려 아이템 크기를 키움 */
  border: 1px solid #ddd;
  border-radius: 8px;
  box-sizing: border-box;
  margin: 0; /* 기존의 margin을 제거 */
}

.board-name {
  font-size: 30px;
  font-weight: bold;
  margin-bottom: 10px; /* 줄과의 간격을 조정 */
  cursor: pointer;
}

.divider {
  height: 2px; /* 줄의 두께 */
  background-color: #FF7315; /* 주황색 */
  margin: 10px 0; /* 줄과 위아래 요소 간의 간격 */
}

.recent-posts {
  list-style: none;
  padding: 0;
}

.recent-posts li {
  margin-bottom: 5px;
  font-size: 16px;
  margin-top: 20px;
  cursor: pointer;
  display: flex; /* 수평으로 배치 */
  justify-content: space-between; /* 양쪽 끝으로 배치 */
}
</style>
