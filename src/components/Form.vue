<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-12">
        <h1 class="text-center">User Information Form</h1>
      </div>
    </div>
    <form @submit.prevent="submitForm">
      <div class="row mb-3">
        <div class="col-md-6 col-sm-12">
          <label for="username" class="form-label">Username</label>
          <input type="text" class="form-control" id="username" @blur="() => validateName(true)" @input="() => validateName(false)" v-model="formData.username">
          <div v-if="errors.username" class="text-danger">{{ errors.username }}</div>
        </div>
        <div class="col-md-6 col-sm-12">
          <label for="password" class="form-label">Password</label>
          <input type="password" class="form-control" id="password" @blur="() => validatePassword(true)" @input="() => validatePassword(false)" v-model="formData.password">
          <div v-if="errors.password" class="text-danger">{{ errors.password }}</div>
        </div>
      </div>
      <div class="row mb-3">
        <div class="col-md-6 col-sm-12">
          <div class="form-check">
            <input type="checkbox" class="form-check-input" id="isAustralian" v-model="formData.isAustralian" @change="validateResident">
            <label class="form-check-label" for="isAustralian">Australian Resident?</label>
          </div>
          <div v-if="errors.resident" class="text-danger">{{ errors.resident }}</div>
        </div>
        <div class="col-md-6 col-sm-12">
          <label for="gender" class="form-label">Gender</label>
          <select class="form-select" id="gender" v-model="formData.gender" @change="validateGender">
            <option value="">Select Gender</option>
            <option value="male">Male</option>
            <option value="female">Female</option>
            <option value="other">Other</option>
          </select>
          <div v-if="errors.gender" class="text-danger">{{ errors.gender }}</div>
        </div>
      </div>
      <div class="mb-3">
        <label for="reason" class="form-label">Reason for joining</label>
        <textarea class="form-control" id="reason" rows="3" v-model="formData.reason" @blur="() => validateReason(true)" @input="() => validateReason(false)"></textarea>
        <div v-if="errors.reason" class="text-danger">{{ errors.reason }}</div>
      </div>
      <div class="text-center">
        <button type="submit" class="btn btn-primary me-2">Submit</button>
        <button type="button" class="btn btn-secondary" @click="clearForm">Clear</button>
      </div>
    </form>
    <div class="mt-5">
      <h3>Submitted Records</h3>
      <DataTable :value="submittedCards" responsiveLayout="scroll" :paginator="true" :rows="5" :rowsPerPageOptions="[5, 10]" :sortMode="'multiple'" :tableStyle="{ minWidth: '50rem' }">
        <Column field="username" header="Username"></Column>
        <Column field="password" header="Password"></Column>
        <Column field="isAustralian" header="Australian Resident?" :body="formatBoolean"></Column>
        <Column field="gender" header="Gender"></Column>
        <Column field="reason" header="Reason for Joining"></Column>
      </DataTable>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';

const formData = ref({
  username: '',
  password: '',
  isAustralian: false,
  reason: '',
  gender: ''
});

const submittedCards = ref([]);

const errors = ref({
  username: null,
  password: null,
  resident: null,
  reason: null,
  gender: null
});
const formatBoolean = (value) => {
  return value ? 'Yes' : 'No';
};

const submitForm = () => {
  validatePassword(true);
  validateName(true);
  validateGender();
  validateReason(true);

  if (!errors.value.username && !errors.value.password && !errors.value.reason && !errors.value.gender) {
    submittedCards.value.push({ ...formData.value });
    clearForm();
  }
};

const clearForm = () => {
  formData.value = {
    username: '',
    password: '',
    isAustralian: false,
    reason: '',
    gender: ''
  };
  errors.value = {
    username: null,
    password: null,
    resident: null,
    reason: null,
    gender: null
  };
};
const validateName = (blur) => {
  if (formData.value.username.length < 3) {
    if (blur) errors.value.username = "Name must be at least 3 characters";
  } else {
    errors.value.username = null;
  }
};

const validatePassword = (blur) => {
  const password = formData.value.password;
  const minLength = 8;
  const hasUppercase = /[A-Z]/.test(password);
  const hasLowercase = /[a-z]/.test(password);
  const hasNumber = /\d/.test(password);
  const hasSpecialChar = /[!@#$%^&*(),.?":{}|<>]/.test(password);

  if (password.length < minLength) {
    if (blur) errors.value.password = `Password must be at least ${minLength} characters long.`;
  } else if (!hasUppercase) {
    if (blur) errors.value.password = "Password must contain at least one uppercase letter.";
  } else if (!hasLowercase) {
    if (blur) errors.value.password = "Password must contain at least one lowercase letter.";
  } else if (!hasNumber) {
    if (blur) errors.value.password = "Password must contain at least one number.";
  } else if (!hasSpecialChar) {
    if (blur) errors.value.password = "Password must contain at least one special character.";
  } else {
    errors.value.password = null;
  }
};

const validateResident = () => {
  if (!formData.value.isAustralian) {
    errors.value.resident = "You must be an Australian resident to join.";
  } else {
    errors.value.resident = null;
  }
};

const validateGender = () => {
  if (!formData.value.gender) {
    errors.value.gender = "Please select a gender.";
  } else {
    errors.value.gender = null;
  }
};

const validateReason = (blur) => {
  if (formData.value.reason.length < 10) {
    if (blur) errors.value.reason = "Reason must be at least 10 characters.";
  } else {
    errors.value.reason = null;
  }
};
</script>

<style scoped>
</style>