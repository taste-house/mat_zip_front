<script setup>
defineProps({
  lists: Array
});
</script>

<template>
  <div>
    <div v-for="(list, index) in lists" :key="index" class="list-item">
      <h3>{{ list.listTitle }} (by {{ list.userNickname }})</h3>
      <p>{{ list.listContent }}</p>
      <span>Likes: {{ list.listLike }}</span>
    </div>
  </div>
</template>

<style scoped>
.list-item {
  padding: 10px;
  border: 1px solid #ddd;
  margin-bottom: 10px;
}
</style>
