<script setup>
import axios from 'axios';
import { ref } from 'vue';

const comments = ref([]);
const errorMessage = ref('');
const loading = ref(false);
const loaded = ref(false);

async function load() {
  try {
    loading.value = true;
    const { data } = await axios.get('https://vue-couseworktwo-default-rtdb.asia-southeast1.firebasedatabase.app/comments.json');
    comments.value = await data[Object.keys(data)[0]];
    errorMessage.value = '';
    loaded.value = true;
  }
  catch {
    errorMessage.value = 'Network error';
  }
  finally {
    loading.value = false;
  }
}
</script>

<template>
  <div class="comments center" v-if="loading === true">
    <span class="loader" ></span>
  </div>
  <div v-else class="comments">
    <div v-if="comments.length === 0 && loaded === false" class="comments__btn">
      <button class="btn btn_dark" @click="load">Load comments</button>
      <span v-if="errorMessage" class="comments__error">{{ errorMessage }}</span>
    </div>
    <div v-else-if="comments.length === 0 && loaded === true" class="card">
      No comments
    </div>
    <div v-else class="card">
      <h3 class="comments__title">Comments</h3>
      <div class="comment" v-for="comment in comments" :key="comment">
        <div class="comment__bold">{{ comment.email }}</div>
        <div>{{ comment.commentary }}</div>
      </div>
    </div>
  </div>

</template>

<style scoped lang='scss'>
.comments {
  grid-column: span 3;

  &__title {
    font-weight: 400;
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
    border-bottom: 1px solid rgb(201, 201, 201);
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

.loader {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  display: inline-block;
  position: relative;
  border: 3px solid;
  border-color: #FFF #FFF transparent;
  animation: rotation 1s linear infinite;

  &::after {
    content: '';
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    margin: auto;
    border: 3px solid;
    border-color: transparent #BDA98B #BDA98B;
    width: 24px;
    height: 24px;
    border-radius: 50%;
    animation: rotationBack 0.5s linear infinite;
    transform-origin: center center;
  }
}

.center {
  display: flex;
  justify-content: center;
}

@keyframes rotation {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}

@keyframes rotationBack {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(-360deg);
  }
}
</style>