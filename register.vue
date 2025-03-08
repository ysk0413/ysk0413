<template>
  <div class="register-page p-mt-4 p-px-3">
    <h2 class="p-text-center">会員登録</h2>
    <RegisterForm />
  </div>
</template>

<script setup>
import RegisterForm from '../components/RegisterForm.vue'
</script>

<style scoped>
.register-page {
  max-width: 600px;
  margin: 0 auto;
}
</style>
