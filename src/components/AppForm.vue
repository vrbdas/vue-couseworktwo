<script setup>
import { ref, watch } from 'vue';

const props = defineProps(['resume']);
const emit = defineEmits(['submit', 'undo']);
const select = ref('title');
const textarea = ref('');
const placeholder = ref('Input here');
const example = ref(false);

function submit() {
  if (!textarea.value.length) {
    return;
  }
  const formData = {
    blockType: select.value,
    blockValue: textarea.value,
  }
  emit('submit', formData);
  textarea.value = '';
}

watch(select, () => {
  if (select.value === 'photo') {
    example.value = true;
    placeholder.value = 'Input image url';
  } else {
    example.value = false;
    placeholder.value = 'Input text here';
  }
});

function exampleCall() {
  textarea.value = 'https://t3.ftcdn.net/jpg/05/70/92/72/360_F_570927210_vhObXb7oDTx5Y4VUu9BUO9pCe7Q0mj09.jpg';
}
</script>

<template>

  <form class="card form" @submit.prevent="submit">
    <label>Block type
      <select name="blockType" v-model="select">
        <option value="title">Title</option>
        <option value="subtitle">Subtitle</option>
        <option value="photo">Photo</option>
        <option value="text">Text</option>
      </select>
    </label>
    <label>
      <div class="space-between">
        Value
        <a v-if="example" class="example" href="#" @click.prevent="exampleCall">example</a>
      </div>
      <textarea name="blockValue" :placeholder="placeholder" v-model.trim="textarea"></textarea>
    </label>
    <div class="space-between">
      <button type="submit" :disabled="!textarea.length" class="btn">Add</button>
      <button type="button" v-if="resume.length" class="btn btn_round" @click="emit('undo')"><svg xmlns="http://www.w3.org/2000/svg" width="16px" height="16px" viewBox="0 0 24 24"><path d="M18.885 3.515C14.268-1.103 6.829-1.161 2.129 3.32L0 1.062V9h7.484L5.418 6.809c2.82-2.706 7.297-2.676 10.073.1C19.832 11.25 17.228 19.2 10 19.2V24c3.708 0 6.614-1.244 8.885-3.515 4.686-4.686 4.686-12.284 0-16.97z"/></svg>
      </button>
    </div>

  </form>
</template>

<style scoped lang='scss'>
.form {
  label {
    display: flex;
    flex-direction: column;
    gap: 5px;
  }

  select,
  textarea {
    background-color: #fff;
    border: 1px solid grey;
    padding: 10px;
    outline: none;
    resize: none;
  }

  textarea {
    min-height: 150px;
  }
}

.textarea {
  display: inline-flex;
  color: grey;
}

.space-between {
  display: flex;
  justify-content: space-between;
}
</style>