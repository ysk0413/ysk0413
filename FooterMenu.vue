<template>
  <footer class="footer-menu p-d-flex p-jc-between p-ai-center">
    <Button label="トップ" icon="pi pi-home" class="p-button-text" @click="goToHome" />
    <Button label="回覧板一覧" icon="pi pi-list" class="p-button-text" @click="goToPosts" />
    <Button label="クーポン一覧" icon="pi pi-tags" class="p-button-text" @click="goToCoupons" />
  </footer>
</template>

<script setup>
import { useRouter } from 'vue-router'
import Button from 'primevue/button'
const router = useRouter()

function goToHome() {
  router.push({ name: 'Home' })
}
function goToPosts() {
  router.push({ name: 'PostsList' })
}
function goToCoupons() {
  router.push({ name: 'CouponsList' })
}
</script>

<style scoped>
.footer-menu {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: #f5f5f5;
  padding: 1rem;
}
</style>
