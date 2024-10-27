<script setup>
import { ref} from 'vue';
import { RouterLink } from 'vue-router';

const isSubMenuVisible = ref([false, false, false, false]);


const toggleSubMenu = (index) => {
  isSubMenuVisible.value[index] = !isSubMenuVisible.value[index];
};

// function toggleSubMenu

</script>

<template>
  <aside class="col-3 col-mg-2 d-md-block sidebar nav-max nav-padding">
    <div class="position-sticky bg-primary-subtle p-3 me-0  ">
      <h5 class="sidebar-heading ">사용자 관리</h5>
      <ul class="nav flex-column ">
        <li class="nav-item">
          <div class="nav-link" @click="toggleSubMenu(0)">회원</div>
          <ul v-if="isSubMenuVisible[0]" class="nav flex-column ms-3">
            <li class="nav-item">
              <RouterLink class="nav-link" to="/admin/users">회원 조회</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink class="nav-link" to="/admin/active-level">활동 등급 관리</RouterLink>
            </li>
          </ul>
        </li>
        <li class="nav-item">
          <a class="nav-link" @click="toggleSubMenu(1)">신고</a>
          <ul v-if="isSubMenuVisible[1]" class="nav flex-column ms-3">
            <li class="nav-item">
              <RouterLink class="nav-link" to="/admin/report-search">신고 조회</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink class="nav-link" to="/admin/report-reason">신고 사유 관리</RouterLink>
            </li>
          </ul>
        </li>
        <li class="nav-item">
          <a class="nav-link" @click="toggleSubMenu(2)">패널티</a>
          <ul v-if="isSubMenuVisible[2]" class="nav flex-column ms-3">
            <li class="nav-item">
              <RouterLink class="nav-link" to="/admin/penalty">패널티 관리</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink class="nav-link" to="/admin/penalty-search">패널티 조회</RouterLink>
            </li>
          </ul>
        </li>
        <li class="nav-item">
          <a class="nav-link" @click="toggleSubMenu(3)">음식점</a>
          <ul v-if="isSubMenuVisible[3]" class="nav flex-column ms-3">
            <li class="nav-item">
              <RouterLink class="nav-link" to   ="/admin/restaurant">음식점 관리</RouterLink>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </aside>

</template>

<style scoped>
.nav-max {
  width: 200px;
}

.nav-padding {
  padding-right: 0;
}
</style>