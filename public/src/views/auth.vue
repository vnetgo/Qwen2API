<template>
  <div class="flex flex-col items-center justify-center w-screen h-screen">
    <transition name="fade-slide">
      <div
        class="flex flex-col items-center w-4/5 h-1/2 bg-opacity-50 bg-white rounded-3xl shadow-xl border-2 border-gray-200 animate-panel"
        v-if="showPanel">
        <h1 class="block mt-24 mb-10 text-2xl font-bold">管理员身份验证</h1>
        <input type="text"
          class="w-4/5 h-16 rounded-2xl bg-opacity-80 bg-white border-2 border-gray-100 pl-10 placeholder:text-gray-500 focus:shadow-lg focus:scale-105 transition-all duration-300"
          placeholder="请输入管理员账号" v-model="apiKey" @keyup.enter="handleLogin">
        <button class="mt-10 w-4/5 h-16 rounded-2xl bg-opacity-65 border-2 border-black bg-black text-white transition-transform duration-200 active:scale-95 hover:scale-105"
          @click="handleLogin">登录</button>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { useRouter } from 'vue-router'

const router = useRouter()
const apiKey = ref('')
const showPanel = ref(false)

const handleLogin = async () => {
  try {
    const res = await axios.post('/verify', {
      apiKey: apiKey.value
    }, {
      headers: {
        'Content-Type': 'application/json'
      }
    })
    if (res.data.status == 200) {     
      localStorage.setItem('apiKey', apiKey.value)
      router.push({ path: '/', replace: true })
    } else {
      alert('apiKey 校验失败,请重新输入!')
    }
  } catch (err) {
    alert('apiKey 校验失败,请重新输入!')
  }
}

onMounted(() => {
  setTimeout(() => {
    showPanel.value = true
  }, 80)
})
</script>

<style lang="css" scoped>
.fade-slide-enter-active, .fade-slide-leave-active {
  transition: opacity 0.5s, transform 0.5s;
}
.fade-slide-enter-from, .fade-slide-leave-to {
  opacity: 0;
  transform: translateY(40px);
}
.fade-slide-enter-to, .fade-slide-leave-from {
  opacity: 1;
  transform: translateY(0);
}
</style>