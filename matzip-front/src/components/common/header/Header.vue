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
}

function handleLogin() {
  router.push('/user/login');
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

</script>

<template>
  <nav id="matzip-header" class="navbar">
    <!-- 아이콘 및 WebName -->
<!--    <div id="web-main">-->
<!--      <img id="matzip-logo" src="@/assets/matzip-logo.png"/>-->
<!--      <a id="navbar-web-name" href="http://localhost:5173/">-->
<!--        MatZip-->
<!--      </a>-->
<!--    </div>-->
    <Logo/>


    <div id="menu-catalog">

      <ul id="menu" class="menu-items">

        <li class="menu-item">
          <b-dropdown id="dropdown-menu" variant="light" offset="25" text="Post" class="m-2" no-caret="">
            <b-dropdown-item>
              <a href="">1번 게시판</a>
            </b-dropdown-item>
            <b-dropdown-item>
              <a href="">2번 게시판</a>
            </b-dropdown-item>
            <b-dropdown-item>
              <a href="">3번 게시판</a>
            </b-dropdown-item>
          </b-dropdown>
        </li>

        <li class="menu-item">
          <b-dropdown id="dropdown-menu" variant="light" offset="25" text="List" class="m-2" no-caret="">
            <b-dropdown-item>
              <a href="http://localhost:5173/user/listAll">전체 리스트</a>
            </b-dropdown-item>
            <b-dropdown-item>
              <a href="">나의 리스트</a>
            </b-dropdown-item>
          </b-dropdown>
        </li>

        <li class="menu-item">
          <b-dropdown id="dropdown-menu" variant="light" offset="25" text="Place" class="m-2" no-caret="">
            <b-dropdown-item>
              <a href="">프로필</a>
            </b-dropdown-item>
            <b-dropdown-item>
              <a href="" @click="checkUserRole">마이페이지</a>
            </b-dropdown-item>
          </b-dropdown>
        </li>

      </ul>


    </div>


    <!-- 인기 태그 노출 -->
    <div id="best-tag">1. 인기 태그</div>

    <b-dropdown text="" variant="link" no-caret="">
      <b-dropdown-item>액션 1</b-dropdown-item>
      <b-dropdown-item>액션 2</b-dropdown-item>
    </b-dropdown>

      <div>
        <button v-if="!authStore.accessToken" @click="handleLogin">로그인</button>
        <button v-if="authStore.accessToken" @click="handleLogout">로그아웃</button>
      </div>

  </nav>
</template>

<style scoped>

a {
  text-decoration: none;
  color: black;
}

.navbar {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  padding: 0;
  border-bottom: 2px solid #ff6f20;
  background-color: #f8f9fa;   /* 배경색 (선택) */
}

/*#web-main {
  display: flex;
  align-items: center;
  justify-content: center;
}

#matzip-logo {
  width: 50px;
  height: 50px;
}

#navbar-web-name {
  font-size: 25px;
  font-weight: bold;
  color: #ff6f20;
}*/


#menu-catalog {
  margin-left: 30px;
}

.menu-items {
  margin: 0;
  padding: 0;
  list-style-type: none;
  display: flex;
  align-items: center;
}

.menu-items > li {
  font-size: 20px;
  cursor: pointer;

}

#best-tag {
  display: flex;
  margin-left: auto;
  color: grey;
}

#dropdown-menu {
  color: #ff6f20;
}




</style>