<script setup>
import { ref, computed } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import axios from 'axios';
import InputField from '@/components/auth/InputField.vue';
import Button from '@/components/auth/Button.vue';

const newPassword = ref('');
const confirmPassword = ref('');
const errorMessage = ref('');
const successMessage = ref('');

const route = useRoute(); // 현재 라우트 정보
const router = useRouter(); // 라우팅 객체
// URL에서 토큰을 추출
const pwToken = route.query.token;

async function resetPassword() {
  console.log('Token:', pwToken);

  // 비밀번호 일치 여부 확인
  if (newPassword.value !== confirmPassword.value) {
    errorMessage.value = '비밀번호가 일치하지 않습니다.';
    return;
  }

  try {
    const response = await axios.post('http://localhost:8000/user/api/v1/auth/reset-password', {
          userPassword: newPassword.value,
        },
        {
          params: {
            token: pwToken, // URL에서 추출한 토큰을 요청에 포함
          },
        }
    );
    successMessage.value = '비밀번호가 성공적으로 변경되었습니다.';
    errorMessage.value = ''; // 에러 메시지 초기화

    // 변경 후 로그인 페이지로 이동
    console.log('비밀번호 변경 성공');
    await router.replace('/login');
  } catch (error) {
    console.error(error);
    if (error.response && error.response.status === 400) {
      // 서버에서 HttpStatus.BAD_REQUEST 반환한 경우
      errorMessage.value = '비밀번호는 영문, 숫자, 특수문자(!@#$%^&*.,? 사용가능)를 모두 포함해 8자리 이상이어야 합니다.';
    } else if(error.response && error.response.status === 404) {
      // 서버에서 HttpStatus.NOT_FOUND를 반환한 경우
      errorMessage.value = '링크의 유효시간이 지났습니다. 다시 진행해주세요.'
    } else {
      errorMessage.value = '비밀번호 변경에 실패했습니다. 다시 시도해 주세요.';
    }
    successMessage.value = ''; // 성공 메시지 초기화
  }
}
</script>

<template>
  <div class="password-reset">
    <h1>비밀번호 재설정</h1>
    <div class="password-reset-form">
      <InputField
          label="새 비밀번호"
          id="new-password"
          placeholder="비밀번호를 입력해주세요"
          v-model="newPassword"
          type="password"
      />
      <InputField
          label="새 비밀번호 확인"
          id="confirm-password"
          placeholder="비밀번호를 다시 입력해주세요"
          v-model="confirmPassword"
          type="password"
      />
      <Button
          @click="resetPassword"
          label="비밀번호 재설정"
          width="100%"
      />
      <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
      <div v-if="successMessage" class="success-message">{{ successMessage }}</div>
    </div>
  </div>
</template>

<style scoped>
.password-reset {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

.password-reset-form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.error-message {
  color: red;
  margin-top: 10px;
}

.success-message {
  color: green;
  margin-top: 10px;
}
</style>
