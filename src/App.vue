<template>
  <div class="container" v-if="showHomeContent">
    <div class="header">
      <h1 class="main-title slide-in">巨一&amp;宾杰</h1>
    </div>
    
    <div class="timer-container slide-up">
      <h2 class="timer-title">我们在一起</h2>
      <div class="timer-display">
        <div class="time-unit">
          <div class="time-value">{{ elapsed.days }}</div>
          <div class="time-label">天</div>
        </div>
        <div class="time-separator">:</div>
        <div class="time-unit">
          <div class="time-value">{{ elapsed.hours }}</div>
          <div class="time-label">时</div>
        </div>
        <div class="time-separator">:</div>
        <div class="time-unit">
          <div class="time-value">{{ elapsed.minutes }}</div>
          <div class="time-label">分</div>
        </div>
        <div class="time-separator">:</div>
        <div class="time-unit">
          <div class="time-value">{{ elapsed.seconds }}</div>
          <div class="time-label">秒</div>
        </div>
      </div>
      <div class="start-date">始于 {{ startDateStr }}</div>
    </div>

    <div class="memory-container slide-up" style="animation-delay: 0.7s;">
      <MemoryEntry />
    </div>
  </div>
  <RouterView />
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue'
import { RouterLink, RouterView } from 'vue-router'
import { useRoute } from 'vue-router'
import backgroundImage from '@/assets/1.jpg'
import MemoryEntry from '@/components/MemoryEntry.vue'

// 戀愛開始時間
const startDate = new Date('2024-03-22')
const startDateStr = '2024.3.22'
const elapsed = ref({ days: 0, hours: 0, minutes: 0, seconds: 0 })

// 計算時間差
const calculateTime = () => {
  const now = new Date()
  const diff = now.getTime() - startDate.getTime()
  
  elapsed.value.days = Math.floor(diff / (1000 * 60 * 60 * 24))
  const hoursDiff = diff % (1000 * 60 * 60 * 24)
  elapsed.value.hours = Math.floor(hoursDiff / (1000 * 60 * 60))
  const minutesDiff = hoursDiff % (1000 * 60 * 60)
  elapsed.value.minutes = Math.floor(minutesDiff / (1000 * 60))
  const secondsDiff = minutesDiff % (1000 * 60)
  elapsed.value.seconds = Math.floor(secondsDiff / 1000)
}

let timer: number

onMounted(() => {
  calculateTime()
  timer = setInterval(calculateTime, 1000)
})

onUnmounted(() => {
  if (timer) clearInterval(timer)
})

const route = useRoute()
const showHomeContent = computed(() => route.name !== 'memory')
</script>

<style scoped>
:global(html, body) {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow: hidden; /* 防止滚动 */
}

/* ✅ 背景图片虚化处理 - 使用伪元素只模糊背景 */
:global(body::before) {
  content: "";
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('@/assets/1.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: fixed;
  filter: blur(10px); /* 只模糊背景 */
  z-index: -2;
}

/* 增加一个半透明遮罩层，增强虚化效果 */
:global(body::after) {
  content: "";
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(255, 255, 255, 0.3);
  z-index: -1;
  pointer-events: none; /* 确保不影响点击 */
}

:global(body) {
  position: relative;
  background: transparent;
}

:global(#app) {
  margin: 0 !important;
  padding: 0 !important;
  width: 100% !important;
  max-width: none !important;
  min-height: 100vh;
  background: transparent !important;
  position: relative;
  z-index: 1;
  display: block !important;            /* 取消 main.css 的 grid 兩欄布局 */
  grid-template-columns: unset !important;
}


/* 容器样式 */
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  position: relative;
  gap: 30px;
}

/* ✅ 让标题在页面顶部水平居中 */
.header {
  text-align: center;
}

.memory-container {
  display: flex;
  justify-content: center;
}

/* ✅ 滑入动画效果 */
@keyframes slideInFromTop {
  from {
    opacity: 0;
    transform: translateY(-50px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slideUpFromBottom {
  from {
    opacity: 0;
    transform: translateY(50px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.slide-in {
  animation: slideInFromTop 1s ease-out forwards;
}

.slide-up {
  animation: slideUpFromBottom 1s ease-out 0.5s forwards;
  opacity: 0;
}

/* 标题样式 - 3D立体效果 */
.main-title {
  font-size: 48px;
  font-weight: bold;
  margin: 0;
  padding: 20px 40px;
  background: linear-gradient(135deg, #6f68ef, #48bedb, #1c89dc); /* 渐变背景 */
  border-radius: 20px; /* 圆角 */
  border: 4px solid rgba(139, 132, 132, 0.8);
  box-shadow: 
    0 10px 30px rgba(0, 0, 0, 0.3),
    0 0 20px rgba(255, 107, 157, 0.5),
    inset 0 2px 10px rgba(255, 255, 255, 0.3),
    inset 0 -2px 10px rgba(0, 0, 0, 0.2);
  color: #ffffff;
  text-shadow: 
    2px 2px 4px rgba(0, 0, 0, 0.3),
    0 0 10px rgba(255, 255, 255, 0.5);
  transition: all 0.3s ease;
  cursor: default;
  transform: perspective(1000px) rotateX(5deg);
  transform-style: preserve-3d;
  position: relative;
  z-index: 10;
  filter: none;
  letter-spacing: 2px;
}

.main-title::before {
  content: "";
  position: absolute;
  top: 5px;
  left: 5px;
  right: 5px;
  height: 50%;
  background: linear-gradient(to bottom, rgba(255, 255, 255, 0.4), transparent);
  border-radius: 15px 15px 0 0;
  pointer-events: none;
  z-index: -1;
}

.main-title:hover {
  color: #1353cb;
  transform: perspective(1000px) rotateX(0deg) scale(1.05);
  box-shadow: 
    0 15px 40px rgba(0, 0, 0, 0.4),
    0 0 30px rgba(11, 112, 221, 0.8),
    inset 0 2px 15px rgba(255, 255, 255, 0.4),
    inset 0 -2px 15px rgba(0, 0, 0, 0.3);
  border-color: rgba(140, 110, 225, 0.9);
}

/* 計時器容器樣式 */
.timer-container {
  text-align: center;
  background: linear-gradient(135deg, rgba(111, 104, 239, 0.8), rgba(72, 190, 219, 0.8));
  padding: 30px 50px;
  border-radius: 30px;
  border: 3px solid rgba(139, 132, 132, 0.6);
  box-shadow: 
    0 10px 30px rgba(0, 0, 0, 0.2),
    0 0 20px rgba(111, 104, 239, 0.4),
    inset 0 2px 10px rgba(255, 255, 255, 0.2),
    inset 0 -2px 10px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  z-index: 10;
  margin-bottom: 0;
}

.timer-title {
  font-size: 24px;
  color: #ffffff;
  margin: 0 0 20px 0;
  text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.3);
  font-weight: bold;
}

.start-date {
  font-size: 18px;
  color: rgba(255, 255, 255, 0.9);
  margin-top: 20px;
  padding: 12px 30px;
  background: rgba(255, 255, 255, 0.15);
  border-radius: 20px;
  border: 2px solid rgba(255, 255, 255, 0.3);
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
  font-weight: 500;
  letter-spacing: 1px;
}

.timer-display {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 15px;
}

.time-unit {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 80px;
}

.time-value {
  font-size: 42px;
  font-weight: bold;
  color: #ffffff;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  background: rgba(255, 255, 255, 0.1);
  padding: 10px 20px;
  border-radius: 15px;
  border: 2px solid rgba(255, 255, 255, 0.3);
  min-width: 80px;
}

.time-label {
  font-size: 16px;
  color: rgba(255, 255, 255, 0.9);
  margin-top: 8px;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
}

.time-separator {
  font-size: 36px;
  font-weight: bold;
  color: rgba(255, 255, 255, 0.8);
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}
</style>
