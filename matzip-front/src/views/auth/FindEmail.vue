<script setup>
import axios from 'axios';
import {onMounted, ref} from 'vue';
import { useRouter } from 'vue-router';
import InputField from '@/components/auth/InputField.vue';
import Button from '@/components/auth/Button.vue';
import Modal from '@/components/auth/Modal.vue';

const name = ref('');
const phone = ref('');
const errorMessage = ref('');
const foundEmail = ref(null);
const isModalOpen = ref(false);
const router = useRouter();

async function findEmail() {
  if (name.value && phone.value) {
    try {
      const response = await axios.post('http://localhost:8000/user/api/v1/auth/find-email', {
        userName: name.value,
        userPhone: phone.value
      });
      // console.log(response.data);

      // API 호출이 성공했을 때 foundEmail에 값을 설정
      foundEmail.value = response.data.message;
      isModalOpen.value = true;
      errorMessage.value = ''; // 오류 메시지 초기화
    } catch (error) {
      // 에러 발생 시 처리
      errorMessage.value = '이메일을 찾을 수 없습니다. 입력한 정보를 확인해 주세요.';
      console.error(error);
    }
  } else {
    errorMessage.value = '이름과 휴대폰 번호를 모두 입력해 주세요.';
  }
}

function navigateToLogin() {
  console.log('로그인으로 이동');
  isModalOpen.value = false;
  router.push('/login');
}

const token = localStorage.getItem('token');

</script>

<template>
  <div class="find-email-page">
    <main class="find-container">
      <h1>이메일/비밀번호 찾기</h1>
      <div class="content-wrapper">
        <nav class="tab-menu">
          <router-link to="/auth/find-email" class="active">이메일 찾기</router-link>
          <router-link to="/auth/find-password">비밀번호 찾기</router-link>
        </nav>

        <div class="find-form">
          <InputField label="이름" placeholder="이름을 입력하세요." v-model="name" />
          <InputField label="휴대폰 번호" placeholder="휴대폰 번호를 입력하세요." v-model="phone" />

          <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>

          <Button @click="findEmail" label="이메일 찾기" width="100%" />

        </div>
      </div>

      <!-- 모달 -->
      <Modal v-if="isModalOpen"  @close="isModalOpen = false">
        <p>이메일을 확인해주세요.</p>

        <p class="privacy-notice">
          개인정보 보호를 위해 직접 입력하지 않은 정보의 경우 일부를 *로 표시하였습니다.
        </p>

        <p class="email-result">{{ foundEmail }}</p>
        <Button @click="navigateToLogin" label="로그인" width="100%" />
      </Modal>
    </main>
  </div>
</template>

<style scoped>
.find-container {
  max-width: 800px;
  margin: 0 auto;
  text-align: center; /* 가운데 정렬 */
}

.content-wrapper {
  display: flex;
  justify-content: center; /* 가로 중앙 정렬 */
}

.tab-menu {
  display: flex;
  flex-direction: column;
  align-items: center; /* 중앙 정렬 */
  margin-right: 20px;
  text-align: left;
}

.tab-menu a {
  margin-bottom: 10px;
  text-decoration: none;
  color: #333;
  font-size: 16px;
}

.tab-menu .active {
  font-weight: bold;
  color: #ff6f20;
}

.find-form {
  max-width: 400px; /* 입력 폼의 최대 너비 설정 */
  flex-grow: 1; /* 폼이 가능한 만큼의 너비를 차지하도록 설정 */
  text-align: left; /* 입력 필드와 버튼 왼쪽 정렬 */
  background: #fff;
  padding: 20px;
  border-radius: 8px;
}

input {
  width: 100%; /* 인풋 필드가 폼의 너비에 맞게 설정 */
  box-sizing: border-box; /* 패딩이 너비에 포함되도록 설정 */
}

.error-message {
  color: red;
  margin-bottom: 10px;
  font-size: 0.9rem;
}

</style>
