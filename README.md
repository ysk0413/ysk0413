<!-- resources/js/components/FooterMenu.vue -->
<template>
  <footer class="footer-menu">
    <button @click="goToHome">トップ</button>
    <button @click="goToPosts">回覧板一覧</button>
    <button @click="goToCoupons">クーポン一覧</button>
  </footer>
</template>

<script setup>
import { useRouter } from 'vue-router'
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
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding: 1rem;
  background-color: #f5f5f5;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
}

.footer-menu button {
  flex: 1;
  border: none;
  background: none;
  padding: 1rem;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.2s;
}

.footer-menu button:hover {
  background-color: #e0e0e0;
}
</style>





<!-- resources/js/App.vue -->
<template>
  <div id="app">
    <HeaderComponent />
    <router-view />
    <FooterMenu />
  </div>
</template>

<script setup>
import HeaderComponent from './components/HeaderComponent.vue'
import FooterMenu from './components/FooterMenu.vue'
</script>
