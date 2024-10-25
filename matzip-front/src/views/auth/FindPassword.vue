<script setup>
import axios from 'axios';
import { ref } from 'vue';
import InputField from '@/components/auth/InputField.vue';
import Button from '@/components/auth/Button.vue';
import Modal from '@/components/auth/Modal.vue';

const email = ref('');
const phone = ref('');
const errorMessage = ref('');
const isModalOpen = ref(false);

async function sendPasswordEmail() {
  if (email.value && phone.value) {
    try {
      const response = await axios.post('http://localhost:8000/user/api/v1/auth/send-pw-reset-url', {
        userEmail: email.value,
        userPhone: phone.value
      });
      console.log(response);

      // api 호출이 성공했을 때
      isModalOpen.value = true;
      errorMessage.value = ''; // 오류 메시지 초기화
    } catch (error) {
      // 에러 발생 시 상태 코드에 따라 메시지 분리
      if (error.response) {
        if (error.response.status === 409) {
          // 서버에서 HttpStatus.CONFLICT를 반환한 경우
          errorMessage.value = '이미 비밀번호 재설정 이메일이 전송되었습니다. 스팸메일함 등을 확인해주세요.';
        } else if (error.response.status === 404) {
          // 서버에서 HttpStatus.NOT_FOUND를 반환한 경우
          errorMessage.value = '가입정보를 찾을 수 없습니다. 입력한 정보를 확인해 주세요.';
        } else {
          // 그 외의 에러
          errorMessage.value = '알 수 없는 오류가 발생했습니다. 잠시 후 다시 시도해 주세요.';
        }
      }
    }
  } else {
    errorMessage.value = '이메일과 휴대폰 번호를 모두 입력해 주세요.';
  }
}

</script>

<template>
  <div class="find-email-page">
    <main class="find-container">
      <h1>이메일/비밀번호 찾기</h1>
      <div class="content-wrapper">
        <nav class="tab-menu">
          <router-link to="/auth/find-email">이메일 찾기</router-link>
          <router-link to="/auth/find-password" class="active">비밀번호 찾기</router-link>
        </nav>

        <div class="find-form">
          <InputField label="이메일" placeholder="가입한 이메일을 입력하세요." v-model="email" />
          <InputField label="휴대폰 번호" placeholder="휴대폰 번호를 입력하세요." v-model="phone" />

          <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>

          <Button @click="sendPasswordEmail" label="비밀번호 재설정" width="100%" />

        </div>
      </div>

      <!-- 모달 -->
      <Modal v-if="isModalOpen"  @close="isModalOpen = false">
        <p>이메일로 비밀번호 재설정 링크를 전송했습니다.</p>

        <p class="expireTime-notice">
          링크는 1시간 동안 유효하니 그 안에 비밀번호를 재설정해주세요.
        </p>

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