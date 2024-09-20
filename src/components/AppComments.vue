<script setup>
import axios from 'axios';
import { ref } from 'vue';

const comments = ref([]);
const errorMessage = ref('');
const loaded = ref(false);

async function load() {
  try {
    const {data} = await axios.get('https://vue-couseworktwo-default-rtdb.asia-southeast1.firebasedatabase.app/comments.json');
    comments.value = await data[Object.keys(data)[0]];
    errorMessage.value = '';
    loaded.value = true;
  }
  catch {
    errorMessage.value = 'Network error';
  }
}
</script>

<template>
  <div v-if="comments.length === 0 && loaded === false" class="comments__btn">
      <button class="btn btn_dark" @click="load">Load comments</button>
      <span v-if="errorMessage" class="comments__error">{{ errorMessage }}</span>
  </div>
  <div v-else-if="comments.length === 0 && loaded === true" class="card comments">
    No comments
  </div>
  <div v-else class="card comments">
    <h2 class="comments__title">Comments</h2>
    <div class="comment" v-for="comment in comments" :key="comment">
      <div class="comment__bold">{{ comment.email }}</div>
      <div>{{ comment.commentary }}</div>
    </div>
  </div>
</template>

<style scoped lang='scss'>
.comments {
  grid-column: span 3;
  gap: 30px;
  &__title {
    font-weight: 600;
    font-size: 2rem;
    margin: 0 auto;
  }
  &__btn {
    display: flex;
    align-items: center;
    gap: 20px;
  }
  &__error {
    color: red;
  }
}

.comment {
  display: flex;
  flex-direction: column;
  position: relative;
  gap: 10px;

  &__bold {
    font-weight: 600;
  }

  &::after {
    content: '';
    border-bottom: 1px solid rgb(209, 209, 209);
    position: absolute;
    bottom: -15px;
    left: 0;
    width: 100%;
  }

  &:last-of-type {
    &::after {
      display: none;
    }
  }

}
</style>