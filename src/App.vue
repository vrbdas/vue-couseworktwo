<script setup>
import AppForm from './components/AppForm.vue';
import AppResume from './components/AppResume.vue';
import AppComments from './components/AppComments.vue';
import axios from 'axios';

import { ref, onMounted } from 'vue';

const resume = ref([]);

onMounted(async () => {
  const {data} = await axios.get('https://vue-couseworktwo-default-rtdb.asia-southeast1.firebasedatabase.app/resume.json');
  for (const key in data) {
    resume.value.push({
      id: key,
      ...data[key],
    });
  }
});

async function formSubmit(formData) {
  const response = await axios.post('https://vue-couseworktwo-default-rtdb.asia-southeast1.firebasedatabase.app/resume.json', JSON.stringify(formData));

  const firebaseData = {
    id: response.data.name,
    ...formData,
  }
  resume.value.push(firebaseData);
}
function undo() {
  axios.delete(`https://vue-couseworktwo-default-rtdb.asia-southeast1.firebasedatabase.app/resume/${resume.value[resume.value.length - 1].id}.json`);
  resume.value.pop();
}
</script>

<template>
  <div class="container">
    <div class="wrapper">
      <AppForm @submit="formSubmit" :resume="resume" @undo="undo"/>
      <AppResume :resume="resume"/>
      <AppComments/>
    </div>
  </div>
</template>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&display=swap');

* {
  box-sizing: border-box;
  font-family: "Inter", sans-serif;
  font-size: 16px;
  font-weight: 400;
}

body {
  background-color: #2c3e50;
  margin: 0;
}

.card {
  background-color: #fff;
  border-radius: 20px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.btn {
    border: none;
    border-radius: 20px;
    padding: 10px 20px;
    min-width: 100px;
    background-color: #0E2134;
    color: #fff;
    cursor: pointer;
    width: fit-content;
    font-weight: 600;
    display: flex;
    justify-content: center;
    align-items: center;
    &:disabled {
      opacity: 0.5;
      cursor: unset;
    }
    &_dark {
      background-color: #BDA98B;
      color: #000;
    }
    svg path {
      fill: #ffffff;
    }
  }
</style>

<style scoped lang='scss'>
.container {
  width: 1320px;
  margin: 0 auto;
}

.wrapper {
  margin-top: 20px;
  display: grid;
  grid-template: auto / repeat(3, 1fr);
  gap: 20px;
}
</style>