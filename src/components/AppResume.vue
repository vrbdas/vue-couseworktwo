<script setup>
const props = defineProps(['resume']);

</script>

<template>
  <div class="card resume">
    <div v-if="resume.length === 0">
      <h3 class="resume__subtitle">Add first block to see the result</h3>
    </div>
    <div v-else>
      <div v-for="(item, i) in resume" :key="i">
        <h2 v-if="item.blockType === 'title'" class="resume__title">{{ item.blockValue }}</h2>
        <h3 class="resume__subtitle" v-if="item.blockType === 'subtitle'">{{ item.blockValue }}</h3>
        <div class="photo" v-if="item.blockType === 'photo'"><img :src="item.blockValue"></div>
        <p v-if="item.blockType === 'text'">{{ item.blockValue }}</p>
      </div>
    </div>
  </div>
</template>
<style scoped lang='scss'>
.resume {
  grid-column: span 2;
  gap: 10px;
  &__title {
    font-weight: 600;
    font-size: 3rem;
    margin: 0;
  }
  &__subtitle {
    font-weight: 400;
    font-size: 2rem;
    margin: 0;
  }
}

p {
  margin: 0;
}

.photo {
  width: 200px;
  height: 200px;
  margin: 0 auto;

  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    border-radius: 100%;
  }
}
</style>