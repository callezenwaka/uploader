<template>
  <form class="upload">
    <h1 class="upload--title">File Uploader</h1>
    <div class="form--item">
      <label for="file" class="form--label required">Upload PDF: </label>
      <input class="form--input" type="file" name="file" id="file" ref="fileRef" @change="handleUpload">
    </div>
    <div class="form--item">
      <img src="" alt="" ref="previewRef">
    </div>
    <div class="form--item">
      <button type="button" class="form--button" :class="{ isValid: isValid }" @click.prevent="handleSubmit">Submit</button>
    </div>
  </form>
</template>

<script setup>
import { computed, ref, reactive } from 'vue';

var previewRef = ref('');
var fileRef = ref('');
var file = ref(null);
var fileName = ref('');

const isValid = computed(() => file.value);

const handleUpload = () => {
  file.value = fileRef.value.files[0];
  fileName.value = fileRef.value.files[0].name;
  const reader = new FileReader();
  return new Promise((resolve, reject) => {
    reader.onerror = () => {
      reader.abort();
      reject(new DOMException("Problem parsing input file."));
    };
    reader.onload = () => {
      resolve(reader.result);
      previewRef.value.src = reader.result;
      previewRef.value.alt = fileRef.value.files[0].name;
    };
    reader.readAsDataURL(fileRef.value.files[0]);
  });
};

const handleReadUpload = async(input) => {
  const reader = new FileReader();
  return new Promise((resolve, reject) => {
    reader.onerror = () => {
      reader.abort();
      reject(new DOMException("Problem parsing input file."));
    };
    reader.onload = () => {
      resolve(reader.result);
      previewRef.value.src = reader.result;
    };
    reader.readAsDataURL(fileRef.value.files[0]);
  });
};

const handleSubmit = async() => {
  const formData = new FormData();
  formData.append("file", file.value, fileName.value);
  // await send(formData);
};

</script>

<style scoped>
.upload {
  padding: 2rem;
}
.upload--title {
  padding: 1rem 0;
  text-align: center;
}
.form--item {
  display: flex;
  flex-direction: column;
  border: none;
  margin: 0px 0px 1.5rem;
  padding: 0px;
}

.form--label {
  color: #000;
  position: relative;
  height: 16px;
  text-align: left;
  font-size: inherit;
  font-weight: bold;
  line-height: 16px;
  letter-spacing: 0.02rem;
}

.form--input {
  height: 3rem;
  width: 100%;
  font-size: inherit;
  line-height: 3rem;
  border-radius: 5px;
  border: 1px solid rgba(0, 0, 0, 0.5);
  outline: none;
  padding-left: 10px;
  margin-top: 5px;
}

.form--button {
  width: 100%;
  margin-top: 16px;
  font-size: 1.2rem !important;
  line-height: 3rem;
  border-radius: 5px;
  border: 2px solid transparent;
  background-color: transparent;
  outline: none;
  cursor: not-allowed;
  padding-left: 10px;
  transition: all 150ms ease-in-out 0s;
  box-shadow: 0 1px 2px 0 rgb(60 64 67 / 30%), 0 1px 3px 1px rgb(60 64 67 / 15%);
}

.form--button.isValid {
  cursor: pointer;
  background-color: #fc8917;
}

.form--button.isValid:hover {
  opacity: 0.5;
}
</style>
