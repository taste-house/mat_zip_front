<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';

const route = useRoute();
const user = ref({}); // 사용자 정보를 저장할 ref

const fetchUserDetail = async () => {
  const userSeq = route.params.userSeq; // URL에서 userSeq 가져오기
  try {
    const response = await axios.get(`http://localhost:8000/user/api/v1/user/${userSeq}`, {
      headers: {
        Authorization: `Bearer ${localStorage.getItem('accessToken')}`
      }
    });
    user.value = response.data.data2; // API 응답을 user에 저장

    console.log(user.value);
  } catch (error) {
    console.error('회원 정보 조회 중 오류 발생:', error);
  }
};

// 컴포넌트가 마운트될 때 사용자 상세 정보 가져오기
onMounted(fetchUserDetail);
</script>

<template>
  <div class="user-detail col">
    <h1>회원 상세조회</h1>
    <table class="table">
      <tbody>
      <tr>
        <th>Email</th>
        <td>{{ user.userEmail }}</td>
      </tr>
      <tr>
        <th>인기회원 / 닉네임</th>
        <td>{{ user.userNickname ? user.userNickname : 'N / 없음' }}</td>
      </tr>
      <tr>
        <th>등급</th>
        <td>{{ user.activityInfo ? user.activityInfo.activeLevelName : '정보 없음' }}</td>
      </tr>
      <tr>
        <th>포인트</th>
        <td>{{ user.activityInfo ? user.activityInfo.activityPoint : 0 }}</td>
      </tr>
      <tr>
        <th>회원가입 날짜</th>
        <td>{{ user.userRegDate }}</td>
      </tr>
      <tr>
        <th>패널티</th>
        <td>{{ user.penaltyStartDate === null ? '-' : user.penaltyStartDate }} ~ {{ user.penaltyEndDate === null ? '-' : user.penaltyEndDate }}</td>
      </tr>
      <tr>
        <th>리스트 수</th>
        <td>{{ user.listCount }}</td>
      </tr>
      <tr>
        <th>리뷰 수</th>
        <td>{{ user.reviewCount }}</td>
      </tr>
      <tr>
        <th>팔로워 수</th>
        <td>{{ user.followerCount }}</td>
      </tr>
      <tr>
        <th>팔로잉 수</th>
        <td>{{ user.followingCount }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.user-detail {
  padding: 20px;
}

.table {
  width: 100%;
  border-collapse: collapse;
}

.table th, .table td {
  padding: 10px;
  border: 1px solid #ccc;
}

.table th {
  background-color: #f2f2f2;
}
</style>