<script setup>
import {onMounted, ref} from 'vue';
import axios from 'axios';
import SelectComponent from '../../../components/admin/admin-views/user-search/SelectComponent.vue';
import TableComponent from "@/components/admin/admin-views/user-search/TableComponent.vue";


const orderBy = ref('');
const selectedOption = ref('');
const influencerYn = ref('');
const searchWord = ref('');

const orderByOptions = [
  {value: 'regDateDesc', text: '가입일 최신순'},
  {value: 'regDateAsc', text: '가입일 오래된순'},
  {value: 'gradeDesc', text: '등급 포인트 높은 순'},
  {value: 'gradeAsc', text: '등급 포인트 낮은 순'},
  {value: 'nicknameDesc', text: '닉네임 내림차순'},
  {value: 'nicknameAsc', text: '닉네임 오름차순'}
]

const searchOptions = [
  {value: 'nickname', text: '닉네임'},
  {value: 'email', text: '이메일'},
];

const influencerOptions = [
  {value: 'Y', text: '인기회원'},
  {value: 'N', text: '일반'}
];


const endPoint = ref('');

const search = async () => {
  const searchParams = {
    searchType: selectedOption.value,
    influencerYn: influencerYn.value,
    searchWord: searchWord.value,
    orderBy: orderBy.value
  };

  endPoint.value = '';

  if (searchParams.searchType !== '') {
    endPoint.value += `?searchType=${searchParams.searchType}`;
  }

  if (searchParams.influencerYn !== '') {
    if (endPoint.value.length > 0) endPoint.value += '&';
    else {
      endPoint.value += '?';
    }
    endPoint.value += `influencerYn=${searchParams.influencerYn}`;
  }

  if (searchParams.searchWord !== '') {
    if (endPoint.value.length > 0) endPoint.value += '&';
    else {
      endPoint.value += '?';
    }
    endPoint.value += `searchWord=${searchParams.searchWord}`;
  }

  if (searchParams.orderBy !== '') {
    if (endPoint.value.length > 0) endPoint.value += '&';
    else {
      endPoint.value += '?';
    }
    endPoint.value += `orderBy=${searchParams.orderBy}`;
  }
  console.log(endPoint.value);
};

</script>
<template>

  <div class="col p-0 m-0">
    <div class="bg-primary-subtle ps-4">
      <span>사용자 관리 > 회원 조회</span>
    </div>
    <div class="container my-4">
      <h1 class="mb-4">회원 조회</h1>

      <div class="row mb-3">
        <div class="col-2">
          <SelectComponent
              label="검색조건"
              id="nickname"
              :options="searchOptions"
              placeholder="검색 조건"
              v-model="selectedOption"
          />
        </div>
        <div class="col-md-5">
          <label for="searchInput" class="form-label">검색</label>
          <input type="text" class="form-control" id="searchInput" v-model="searchWord" placeholder="검색어 입력">
        </div>
        <div class="col-md-3 d-flex align-items-end">
          <button class="btn btn-secondary" @click="search">검색</button>
        </div>
      </div>


      <div class="row mb-3">
        <SelectComponent
            label="인기회원"
            id="influencerYn"
            :options="influencerOptions"
            placeholder="인기회원 선택..."
            v-model="influencerYn"
        />
        <SelectComponent
            label="정렬"
            id="orderBy"
            :options="orderByOptions"
            placeholder=""
            v-model="orderBy"
        />
      </div>

    </div>

    <div>
      <TableComponent :endPoint="endPoint"/>
    </div>
  </div>


</template>

<style scoped>

</style>