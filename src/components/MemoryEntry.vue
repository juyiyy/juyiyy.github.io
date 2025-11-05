<template>
  <div class="memory-card" @click="go">
    <div class="memory-title">回忆录</div>
    <div class="memory-sub">点击进入</div>
  </div>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router'
const router = useRouter()
const go = () => {
  router.push({ name: 'memory' })
}
</script>

<style scoped>
.memory-card{
  cursor: pointer;
  user-select: none;
  padding: 30px 36px;
  border-radius: 24px;
  border: 2px solid rgba(255,255,255,0.35);
  background: linear-gradient(135deg, rgba(255,255,255,.2), rgba(255,255,255,.05));
  backdrop-filter: blur(10px);
  box-shadow: 0 10px 30px rgba(0,0,0,.2), inset 0 2px 10px rgba(255,255,255,.2);
  color: #4f8dcb;
  text-align: center;
  transition: transform .2s ease, box-shadow .2s ease, border-color .2s ease;
  min-width: 230px;
  min-height: 110px;
}
.memory-card:hover{
  transform: translateY(-4px);
  box-shadow: 0 16px 40px rgba(0,0,0,.3), 0 0 24px rgba(111,104,239,.45);
  border-color: rgba(111,104,239,.7);
}
.memory-title{ font-size: 28px; font-weight: 700; letter-spacing: 2px; }
.memory-sub{ font-size: 16px; opacity: .9; margin-top: 8px; }
</style>

