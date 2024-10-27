<template class="main-template">
  <div class="main-container">


    <img src="../../assets/space.png" class="space-text"/>

    <!--     맵 & 인기태그-->
    <div class="main-map">
      <current-map />
      <hot-tag :tags="tags" />
    </div>

<!--  인기 리스트  -->
    <div class="main-list">
      <hot-list :items="hotListItem" />
    </div>

<!--  게시판  -->
    <div class="main-post">
      <home-post :boards="boards" />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import HotTag from '@/components/main/HotTag.vue';
import CurrentMap from '@/components/main/CurrentMap.vue';
import HotList from '@/components/main/HotList.vue';
import HomePost from '@/components/main/HomePost.vue';

// 인기 태그 배열 선언
const tags = ref([]);
const errorMessage = ref(null);
// 인기 리스트 배열 선언
const hotListItem = ref([]);
//게시판 배열 선언
const boards = ref([]);


// 인기 태그 마운트 될때 데이터 가져오기
onMounted(async () =>{
  try{
    // 인기 태그 가져오기
    const tagResponse = await axios.get('https://matzipapi.huichan.kr/back/api/v1/posts/top-10-tags');
    if(tagResponse.data && tagResponse.data.data2 && tagResponse.data.data2.tags){
      tags.value = tagResponse.data.data2.tags;
    }else{
      tags.value = []; // 데이터 없을때 빈 배열 나오게 설정
      errorMessage.value= '태그가 없습니다.'
    }

    // 인기 리스트 가져오기
    const listResponse = await axios.get('https://matzipapi.huichan.kr/back/api/v1/listbox/popular')
    if(listResponse.data && listResponse.data.data){
      hotListItem.value = listResponse.data.data;

    }else{
      hotListItem.value = [];
      errorMessage.value='리스트가 없습니다.'
      console.log(hotListItem.value);
    }


    // 게시판 데이터 가져오기
    const page=1;
    const size=5;
    const maxBoardSeq = 5; // 최대 게시판 번호 지정

    for (let boardSeq = 1; boardSeq <= maxBoardSeq; boardSeq++){
      try{
        const postResponse = await axios.get(`https://matzipapi.huichan.kr/back/api/v1/boards/${boardSeq}/posts`, {
          params: { page, size }
        });

        // 게시판이 존재하는 경우에만 데이터 추가
        if (postResponse.data.data2 && postResponse.data.data2.posts) {
          const recentPosts = postResponse.data.data2.posts.map(post => ({
            seq : post.postSeq,
            title: post.postTitle,
            userNickname: post.userNickname,
            boardCategoryName: post.boardCategoryName
          }));

          // 해당 boardSeq에 대한 정보 추가
          boards.value.push({
            name: recentPosts[0].boardCategoryName,
            recentPosts: recentPosts

          });
        }
      } catch(error){
        console.log(`Board ${boardSeq} 데이터 가져오기 실패:`, error);
      }
    }
  } catch(error){
    console.error('데이터 가져오기 실패:', error);
    errorMessage.value = error.response ? error.response.data.message : error.message; // 에러메세지 저장
  }
});


</script>

<style scoped>
.main-container {
  display: flex;
  flex-direction: column; /* 수직 정렬을 위해 열 방향으로 설정 */
  justify-content: center; /* 수평 가운데 정렬 */
  align-items: center; /* 수직 가운데 정렬 */
}

.main-map {
  display: flex;
  width: 100%;
  border: 1px solid #ccc; /* 회색 테두리 추가 */
  border-radius: 13px; /* 테두리 곡률 설정 */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2); /* 그림자 추가 */
  padding: 16px; /* 내부 여백 추가 */
  background-color: #fff; /* 배경색 추가 */

}

.space-text {
  width: 160px;
  margin: 40px 0 20px 10px;
  align-self: flex-start; /* 왼쪽 정렬 */
}




</style>