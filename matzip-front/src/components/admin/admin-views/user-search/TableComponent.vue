<script setup>
import { ref, watch, computed } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

const props = defineProps({
  endPoint: {
    type: String,
    required: true
  }
});

const members = ref([]);
const currentPage = ref(1);
const itemsPerPage = 10; // 페이지당 아이템 수

// 전체 페이지 수 계산
const totalPages = computed(() => Math.ceil(members.value.length / itemsPerPage));

// 데이터 조회 함수
const fetchMembers = async () => {
  try {
    const response = await axios.get(`http://localhost:8000/user/api/v1/users${props.endPoint}&page=${currentPage.value}`, {
      headers: {
        Authorization: `Bearer ${localStorage.getItem('accessToken')}`
      }
    });

    members.value = response.data.data2.userLists; // 데이터 저장

    // 페이지 조정: 데이터가 없으면 현재 페이지를 감소
    if (members.value.length === 0 && currentPage.value > 1) {
      currentPage.value--;
      await fetchMembers(); // 유효한 페이지로 데이터 조회
    }
  } catch (error) {
    console.error('검색 중 오류 발생:', error);
  }
};

// 페이지 변경 함수
const nextPage = async () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++;
    await fetchMembers(); // 새로운 페이지 데이터 조회
  }
};

const prevPage = async () => {
  if (currentPage.value > 1) {
    currentPage.value--;
    await fetchMembers(); // 새로운 페이지 데이터 조회
  }
};

// watch를 사용하여 endPoint가 변경될 때 fetchMembers 호출
watch(() => props.endPoint, (newEndPoint) => {
  if (newEndPoint) {
    currentPage.value = 1; // endPoint 변경 시 첫 페이지로 초기화
    fetchMembers(); // 새로운 endPoint로 데이터 조회
  }
});

watch(() => currentPage, (newPage) => {
  if (currentPage) {
    fetchMembers();
  }
})

// Vue Router 사용
const router = useRouter();

function goDetail(userSeq) {
  // userSeq를 사용하여 상세 조회 페이지로 이동
  router.push(`/admin/user/${userSeq}`);
}


</script>

<template>
  <table class="table table-bordered">
    <thead class="thead-light">
    <tr>
      <th>인기 회원</th>
      <th>유저번호</th>
      <th>이메일</th>
      <th>닉네임</th>
      <th>포인트</th>
      <th>등급</th>
      <th>가입일</th>
      <th>패널티</th>
      <th>페널티 부여 여부</th>
    </tr>
    </thead>
    <tbody>
    <tr v-for="member in members" :key="member.userSeq">
      <td>
        <span class="text-danger" v-if="member.activityInfo && member.activityInfo.influencerYn === 'Y'">&#x2764;</span>
      </td>
      <td>{{ member.userSeq }}</td>
      <td>{{ member.userEmail }}</td>
      <td>{{ member.userNickname }}</td>
      <td>{{ member.activityInfo ? member.activityInfo.activityPoint + '점' : '0점' }}</td>
      <td>{{ member.activityInfo ? member.activityInfo.activeLevelName : '정보 없음' }}</td>
      <td>{{ member.userRegDate }}</td>
      <td>{{ member.penalty ? member.penalty : '기록 없음' }}</td>
      <td>{{ member.penaltyYn }}</td>
      <td>
        <button class="btn btn-info" @click.prevent="goDetail(member.userSeq)">조회</button>
      </td>
    </tr>
    </tbody>
  </table>
  <div class="pagination-container">
    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">이전</button>
      <span>페이지 {{ currentPage }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">다음</button>
    </div>
  </div>
</template>

<style scoped>
.pagination-container {
  display: flex;
  justify-content: center; /* 중앙 정렬 */
  margin-top: 20px; /* 여백 추가 */
}

.pagination button {
  margin: 0 5px; /* 버튼 사이 여백 */
}
</style>