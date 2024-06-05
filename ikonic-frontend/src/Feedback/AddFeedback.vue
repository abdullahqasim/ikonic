<template>
  <App>
    <div class="w-full md:w-[800px] mx-auto mt-6 px-6 py-4 bg-white shadow-lg sm:rounded-lg">
      <div class="w-full mt-6 px-6">
        <p v-for="(error, index) in errors" :key="index" class="text-red-500 text-sm">
          {{ error.message }}
        </p>

        <form @submit.prevent="addFeedback">
          <div class="grid grid-cols-1 md:grid-cols-2 gap-2 mb-4">
            <InputField title="Title" placeholder="Enter title" type="text" name="title" id="title" :maxlength="150"
              isRequired @update:modelValue="setInputValue" alreadyAccount="true" :error="error.title" />

            <Dropdown title="Category" :options="categories" value="" @update:modelValue="setInputValue" name="category"
              isRequired :error="error.category" />
          </div>

          <label for="description" class="mt-4">Description
            <Staric />
          </label>
          <QuillEditor theme="snow" id="description" v-model:content="formObj.description"
            style="min-height: 200px !important; max-height: 200px;" contentType="html" />
          <InputError :error="error.description" />

          <div class="flex items-center justify-end mt-4">
            <Btn :text="'Submit'" :isDisabled="isDisabled" />
          </div>
        </form>
      </div>
    </div>
  </App>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
import App from '../Layouts/Dashboard/App.vue';
import Btn from '../components/GlobalCompnents/Btn.vue';
import InputField from '../components/GlobalCompnents/InputField.vue';
import Staric from '../components/GlobalCompnents/Staric.vue';
import InputError from '../components/GlobalCompnents/InputError.vue';
import Dropdown from '../components/GlobalCompnents/Dropdown.vue';

const router = useRouter();

const errors = ref([]);
const categories = ref([
  { id: 1, name: 'Bug Report' },
  { id: 2, name: 'Feature Request' },
  { id: 3, name: 'Other Improvements' },
]);
const isDisabled = ref(false);

const formObj = ref({
  title: '',
  category: '',
  description: '',
});

const error = ref({
  title: '',
  category: '',
  description: '',
});

const errorProperty = ref({
  title: false,
  category: false,
  description: false,
});

function setInputValue(obj) {
  formObj.value[obj.property] = obj.value;
  console.log(formObj.value);
}

function validateForm() {
  let valid = true;

  if (!formObj.value.title) {
    error.value.title = 'Title is required';
    errorProperty.value.title = true;
    valid = false;
  } else {
    error.value.title = '';
    errorProperty.value.title = false;
  }

  if (!formObj.value.category) {
    error.value.category = 'Category is required';
    errorProperty.value.category = true;
    valid = false;
  } else {
    error.value.category = '';
    errorProperty.value.category = false;
  }

  if (!formObj.value.description) {
    error.value.description = 'Description is required';
    errorProperty.value.description = true;
    valid = false;
  } else {
    error.value.description = '';
    errorProperty.value.description = false;
  }

  return valid;
}

async function addFeedback() {
  if (!validateForm()) {
    isDisabled.value = false;
    return;
  }

  isDisabled.value = true;

  const headers = {
    Accept: 'application/json',
    'Content-Type': 'multipart/form-data',
    'Access-Control-Allow-Origin': '*',
  };

  const token = JSON.parse(localStorage.getItem('user-token'));

  if (token) {
    headers.Authorization = `Bearer ${token}`;
  }

  try {
    const response = await axios.post(`${import.meta.env.VITE_BASE_URL}/add-feedback`, formObj.value, { headers });
    if (response.data.success) {
      router.push({ name: 'feedback' });
    } else {
      errors.value = response.data.errors;
      isDisabled.value = false;
    }
  } catch (error) {
    console.error('An error occurred:', error);
    isDisabled.value = false;
  }
}
</script>
