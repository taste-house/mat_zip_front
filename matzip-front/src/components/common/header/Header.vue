<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import Logo from '@/components/common/Logo.vue';
import { useAuthStore } from '@/components/stores/auth.js';
import {useRouter} from "vue-router";

const authStore = useAuthStore();
const router = useRouter();

function checkUserRole() {
  if (authStore.userRole !== 'admin') router.push('/admin');
  else if (authStore.userRole !== 'user') router.push(`/user/${authStore.userSeq}`);
  else {
    router.push('/login');
  }

}

function handleLogin() {
  router.push('/login');
}

function handleLogout() {
  authStore.logout();
}

function handleClickOutside(event) {
  const dropdowns = document.querySelectorAll('.dropdown');

  if (activeMenu.value) {
    const isClickInsideDropdown = Array.from(dropdowns).some(dropdown =>
        dropdown.contains(event.target)
    );

    if (!isClickInsideDropdown) {
      activeMenu.value = null; // 드롭다운 외부 클릭 시 숨김
    }
  }
}

function handleMenuItemClick() {
  activeMenu.value = null; // 메뉴 항목 클릭 시 드롭다운 숨김
}

// onMounted 와 onBeforeUnmount 훅 사용
onMounted(() => {
  console.log(localStorage.getItem('accessToken'));
  document.addEventListener('click', handleClickOutside); // 외부 클릭 리스너 추가

  // 메뉴 항목 클릭 리스너 추가
  const menuItems = document.querySelectorAll('.dropdown .menu-item');
  menuItems.forEach(item => {
    item.addEventListener('click', handleMenuItemClick);
  });
});

onBeforeUnmount(() => {
  document.removeEventListener('click', handleClickOutside); // 컴포넌트 언마운트 시 리스너 제거

  // 메뉴 항목 클릭 리스너 제거
  const menuItems = document.querySelectorAll('.dropdown .menu-item');
  menuItems.forEach(item => {
    item.removeEventListener('click', handleMenuItemClick);
  });
});
console.log(localStorage.getItem('accessToken'));
</script>

<template>
  <b-navbar id="matzip-header" class="navbar navbar-expand-lg">
    <b-navbar-brand href="#">
      <Logo />
    </b-navbar-brand>
    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
    <b-collapse id="nav-collapse" is-nav>
      <div class="d-flex justify-content-between w-100">
        <b-navbar-nav>
          <b-nav-item href="/user/post">Post</b-nav-item>
          <b-nav-item-dropdown text="List">
            <b-dropdown-item href="/user/listAll">전체 리스트</b-dropdown-item>
            <b-dropdown-item href="#" disabled>나의 리스트</b-dropdown-item>
          </b-nav-item-dropdown>
          <b-nav-item href="/user/review">Review</b-nav-item>
          <b-nav-item @click="checkUserRole">MyPage</b-nav-item>
        </b-navbar-nav>
        <b-navbar-nav class="ml-auto">
          <b-nav-item id="best-tag">1. 인기 태그</b-nav-item>
          <button class="customButton" v-if="!authStore.accessToken" @click="handleLogin">로그인</button>
          <button class="customButton" v-if="authStore.accessToken" @click="handleLogout">로그아웃</button>
        </b-navbar-nav>
      </div>
    </b-collapse>
  </b-navbar>
</template>

<style scoped>

.navbar {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  padding: 0;
  border-bottom: 2px solid #ff6f20;
  background-color: #f8f9fa;   /* 배경색 (선택) */
}

.d-flex {
  margin-left: 20px;
}

.nav-item {
  font-size: large;
  font-weight: bold;
  min-width: 75px;
}

#best-tag {
  font-weight: lighter;
}

.customButton {
  background-color: #ff6f20; 
  color: white;
  text-align: center;
  text-decoration: none;
  border-radius: 8px;
  margin-left: 30px;
  margin-right: 10px;
  min-width: 75px;
}
.customButton:hover {
  background-color: #e65a00; 
}

</style>
