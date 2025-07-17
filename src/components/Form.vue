<template>
  <div class="container mt-5">
    <div class="row justify-content-center">
      <div class="col-12 col-md-10 col-lg-8">
        <h1 class="text-center">User Information Form</h1>
        <form @submit.prevent="submitForm">
          <div class="row mb-3">
            <div class="col-12 col-sm-6">
              <label for="username" class="form-label">Username</label>
              <input type="text" class="form-control" id="username" v-model="formData.username"
                @blur="() => validateName(true)"
                @input="() =>validateName(false)" />
              <div v-if="errors.username" class="text-danger">{{ errors.username }}</div>
            </div>
            <div class="col-12 col-sm-6 mt-3 mt-sm-0">
              <label for="password" class="form-label">Password</label>
              <input type="password" class="form-control" id="password" v-model="formData.password"
              @blur="() => validatePassword(true)"
              @input="() => validatePassword(false)" />
              <div v-if="errors.password" class="text-danger">{{ errors.password }}</div>
            </div>
          </div>
          <div class="row mb-3">
            <div class="col-12 col-sm-6">
              <div class="form-check">
                <input type="checkbox" class="form-check-input" id="isAustralian" v-model="formData.isAustralian" @change="() => validateIsAustralian(true)">
                <label class="form-check-label" for="isAustralian">Australian Resident?</label>
              </div>
              <div v-if="errors.isAustralian" class="text-danger">{{ errors.isAustralian }}</div>
            </div>
            <div class="col-12 col-sm-6 mt-3 mt-sm-0">
              <label for="gender" class="form-label">Gender</label>
              <select class="form-select" id="gender" v-model="formData.gender" @change="() => validateGender(true)">
                <option disabled value="">Select Here...</option>
                <option value="male">Male</option>
                <option value="female">Female</option>
                <option value="other">Others</option>
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
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, defineEmits } from 'vue';

const emit = defineEmits(['submit']);

const initialFormData = {
    username: '',
    password: '',
    isAustralian: false,
    reason: '',
    gender: ''
};

const formData = ref({ ...initialFormData });

const submitForm = () => {
  validateName(true);
  validatePassword(true);
  validateIsAustralian(true);
  validateGender(true);
  validateReason(true);
  if (!errors.value.username && !errors.value.password && !errors.value.isAustralian && !errors.value.gender && !errors.value.reason) {
    emit('submit', { ...formData.value });
    clearForm();
  }
};

const clearForm = () => {
    formData.value = { ...initialFormData };
    errors.value = {
        username: '',
        password: '',
        isAustralian: '',
        reason: '',
        gender: ''
    };
};

const errors = ref({
    username: '',
    password: '',
    isAustralian: '',
    reason: '',
    gender: ''
});

const validateName = (blur) => {
    if (formData.value.username.length < 3) {
      if(blur) errors.value.username = 'Name must be at least 3 characters';
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

const validateIsAustralian = (blur) => {
    if (!formData.value.isAustralian) {
      if(blur) errors.value.isAustralian = 'You must be an Australian resident.';
    } else {
      errors.value.isAustralian = null;
    }
};

const validateGender = (blur) => {
    if (!formData.value.gender) {
      if(blur) errors.value.gender = 'Please select a gender.';
    } else {
      errors.value.gender = null;
    }
};

const validateReason = (blur) => {
    if (!formData.value.reason.trim()) {
      if(blur) errors.value.reason = 'Reason for joining is required.';
    } else {
      errors.value.reason = null;
    }
};

</script>

<style scoped>
/* Our logic will go here */
   .card {
   border: 1px solid #ccc;
   border-radius: 10px;
   box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
   }
   .card-header {
   background-color: #275FDA;
   color: white;
   padding: 10px;
   border-radius: 10px 10px 0 0;
   }
   .list-group-item {
   padding: 10px;
   }
</style>