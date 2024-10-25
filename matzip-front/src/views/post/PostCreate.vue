<script setup>
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";
import Editor from "@toast-ui/editor";
import "@toast-ui/editor/dist/toastui-editor.css"; // Editor's Style
import axios from "axios";

const router = useRouter();
const editor = ref(null);
const currentUserSeq = ref(null);

const formData = ref({
  boardCategorySeq: 1,
  postTitle: "",
  postContent: "",
  listSeq: null,
  restaurantSeq: null,
  tagName: "",
})

/* 에디터를 초기화하는 함수 */
const initializeEditor = () => {
  editor.value = new Editor({
    el: document.querySelector('#editor'),  // 에디터를 적용할 요소 (컨테이너)
    height: '500px',                        // 에디터 영역의 높이 값 (00px || auto)
    initialEditType: 'wysiwyg',             // 최초로 보여줄 에디터 타입 (markdown || wysiwyg)
    previewStyle: 'vertical',               // 마크다운 프리뷰 스타일 (tab || vertical)
    hooks: {
      addImageBlobHook: async (blob, callback) => {
        const uploadResult = await uploadImage(blob);
        callback(`https://matzipapi.huichan.kr/back${uploadResult.url}`);
      }
    }
  });
};

/* 이미지 업로드 함수 */
const uploadImage = async (blob) => {
  const imageFormData = new FormData();
  imageFormData.append('image', blob);

  try {
    const response = await axios.post('https://matzipapi.huichan.kr/back/api/v1/posts/uploadImage', imageFormData);
    return response.data;
  } catch (error) {
    console.error('Image upload failed:', error);
    throw error;
  }
};

/* 게시글 제출 함수 */
const submitPost = async () => {
  const htmlContent = editor.value.getHTML();
  formData.value.postContent = htmlContent; // 에디터 내용을 formData에 추가

  // tags는 단어를 쉼표로 나누고 공백 제거하여 배열로 변환
  formData.value.tagName = formData.value.tagName.split(',')
      .map(tag => tag.trim())
      .filter(tag => tag); // 빈 태그 제거

  // 새로운 객체를 생성하여 순환 참조를 방지
  const dataToSend = {
    ...formData.value, // 전개 연산자를 사용하여 새로운 객체로 복사
  };

  // 게시글 데이터를 서버에 전송하는 코드 작성
  try {
    await axios.post('https://matzipapi.huichan.kr/back/api/v1/posts', dataToSend,{
      headers: {
        Authorization: `Bearer ${localStorage.getItem('accessToken')}`, // localStorage에서 토큰 읽기
        "Content-Type": "application/json",
      },
    });
    router.push('/user/post'); // 게시글 목록 페이지로 리다이렉트
  } catch (error) {
    console.error('Post submission failed:', error);
  }
};

/* 컴포넌트 마운트 시 에디터 초기화 */
onMounted(() => {
  initializeEditor();

  const token = localStorage.getItem('accessToken');
  if (token) {
    const payload = JSON.parse(atob(token.split('.')[1]));  // JWT 토큰의 페이로드 추출
    currentUserSeq.value = payload.sub; // 사용자 ID 저장
  }
});

</script>

<template>
  <form @submit.prevent="submitPost" class="editor container">

    <div class="form group">
      <label for="boardCategory" class="form-label">게시판 카테고리</label>
      <input type="number" id="boardCategory" v-model="formData.boardCategorySeq" class="form-control" required/>
    </div>

    <div class="form-group">
      <label for="postTitle" class="form-label">게시글 제목</label>
      <input type="text" id="postTitle" v-model="formData.postTitle" class="form-control" required/>
    </div>

    <div class="form-group" id="editor">
      <label for="postContent" class="form-label"></label>
      <input type="text" id="editor" v-model="formData.postContent" class="form-control" required/>
    </div>

    <div class="form group">
      <label for="list" class="form-label">리스트</label>
      <input type="number" id="list" v-model="formData.listSeq" class="form-control"/>
    </div>

    <div class="form group">
      <label for="restaurant" class="form-label">음식점</label>
      <input type="number" id="restaurant" v-model="formData.restaurantSeq" class="form-control"/>
    </div>

    <div class="form group">
      <label for="tag" class="form-label">태그</label>
      <input type="text" id="tag" v-model="formData.tagName" class="form-control"/>
    </div>

    <button type="submit">글쓰기완료</button>
  </form>
</template>

<style scoped>
/* 스타일 추가 필요 시 작성 */
</style>
