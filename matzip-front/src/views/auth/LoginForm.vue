<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';
import Logo from '@/components/common/Logo.vue';
import InputField from '@/components/auth/InputField.vue';
import Button from '@/components/auth/Button.vue';
import {useAuthStore} from "@/components/stores/auth.js";

const authStore = useAuthStore();
const email = ref('');
const password = ref('');
const errorMessage = ref('');
const router = useRouter();

async function handleLogin() {
  try {
    if(email.value === '' || email.value == null) {
      errorMessage.value = '이메일을 입력해 주세요';
      return;
    }
    if(password.value === '' || password.value == null) {
      errorMessage.value = '비밀번호를 입력해 주세요';
      return;
    }

    const response = await axios.post('http://localhost:8000/user/api/v1/auth/login', {
      userEmail: email.value,
      userPassword: password.value,
    });

    // jwt 토큰 가져오기
    const token = response.headers.token;

    // authStore의 login 메소드로 토큰 저장
    authStore.login(token);
    authStore.token = token;

    await router.replace('/');
  } catch (error) {
    errorMessage.value = '이메일 또는 비밀번호가 잘못 되었습니다.<br> 이메일과 비밀번호를 정확히 입력해 주세요';
  }
}

</script>

<template>
  <div class="login-page">
    <main class="login-container">
      <Logo size="150px"/>
      <div class="login-form">
        <div class="login-form-inputs">
          <InputField label="Email" id="email" placeholder="이메일을 입력하세요" v-model="email" class="left-aligned-label" />
          <InputField label="Password" id="password" type="password" placeholder="비밀번호를 입력하세요" v-model="password" class="left-aligned-label" />
        </div>

        <div v-if="errorMessage" class="error-message"  v-html="errorMessage"></div>

        <Button @click="handleLogin" label="로그인" width="100%" height="40px" />

        <div class="login-links">
          <router-link to="/auth/register" class="signup-link">회원가입</router-link>
          <div class="find-links">
            <router-link to="/auth/find-email">Email 찾기</router-link>
            <span class="separator">|</span>
            <router-link to="/auth/find-password">Password 찾기</router-link>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
.login-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.login-form {
  max-width: 400px;
  width: 100%;
  text-align: center;
}

.login-form-inputs {
  margin-bottom: 15px;
}

.login-links {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
}

.signup-link, .find-links a {
  text-decoration: none;
  color: #333;
  font-size: 14px;
}

.find-links a:hover, .signup-link:hover {
  color: #ff6f20;
}

.separator {
  margin: 0 5px;
  color: #aaa;
}

.error-message {
  color: red;
  margin-bottom: 10px;
  font-size: 0.9rem;
}

.login-form-inputs label {
  display: block;
  text-align: left;
  margin-bottom: 5px;
}
</style>
