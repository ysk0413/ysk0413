<template>
  <div class="p-text-center" style="margin-top: 2rem;">
    <h2>未登録</h2>
    <p>まだ会員登録がお済みではありません。</p>
    <Button label="会員登録する" icon="pi pi-user-plus" @click="goToRegister" class="p-mt-2" />
  </div>
</template>

<script setup>
import { useRouter } from 'vue-router'
import Button from 'primevue/button'

const router = useRouter()

function goToRegister() {
  router.push({ name: 'Register' })
}
</script>

<style scoped>
</style>
