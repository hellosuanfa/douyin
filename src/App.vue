<template>
  <div class="device-wrapper">
    <div class="phone-frame">
      <div class="phone-screen">
        <div class="page-container">
          <!-- 顶部导航 - 固定不动 -->
          <div class="top-nav">
            <span class="tab">团购</span>
            <span class="tab active">上海</span>
            <span class="tab">关注</span>
            <span class="tab">商城</span>
            <span class="tab">推荐</span>
          </div>

          <!-- 中间内容区 - 可滑动切换 -->
          <div class="content-wrapper" ref="container">
            <Transition :name="slideDirection" mode="out-in">
              <component :is="currentComponent" :key="currentPage" @swipe="handleSwipe" />
            </Transition>
          </div>

          <!-- 底部导航 - 固定不动 -->
          <div class="bottom-nav">
            <span class="nav-label">首页</span>
            <span class="nav-label">朋友</span>
            <div class="add-box">+</div>
            <span class="nav-label">消息</span>
            <span class="nav-label">我</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, shallowRef, markRaw } from 'vue'
import { useSwipe } from '@vueuse/core'
import NightPage from './views/NightPage.vue'
import MorningPage from './views/MorningPage.vue'
import VideoPage1 from './views/VideoPage1.vue'
import VideoPage2 from './views/VideoPage2.vue'
import VideoPage3 from './views/VideoPage3.vue'
import VideoPage4 from './views/VideoPage4.vue'

const container = ref(null)
const currentPage = ref(0)
const slideDirection = ref('slide-up')

const pages = [markRaw(VideoPage1), markRaw(VideoPage3), markRaw(NightPage), markRaw(VideoPage4), markRaw(VideoPage2), markRaw(MorningPage)]
const currentComponent = computed(() => pages[currentPage.value])

const { direction } = useSwipe(container, {
  onSwipeEnd(e, dir) {
    if (dir === 'up') {
      slideDirection.value = 'slide-up'
      currentPage.value = (currentPage.value + 1) % pages.length
    }
    if (dir === 'down') {
      slideDirection.value = 'slide-down'
      currentPage.value = (currentPage.value - 1 + pages.length) % pages.length
    }
  }
})

const handleSwipe = () => {
  slideDirection.value = 'slide-up'
  currentPage.value = (currentPage.value + 1) % pages.length
}
</script>

<style scoped>
.device-wrapper {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #1a1a2e;
}

.phone-frame {
  width: 375px;
  height: 812px;
  background: #111;
  border-radius: 44px;
  padding: 8px;
  box-shadow: 0 0 60px rgba(0,0,0,0.8);
}

.phone-screen {
  width: 100%;
  height: 100%;
  background: #000;
  border-radius: 38px;
  overflow: hidden;
}

.page-container {
  height: 100%;
  display: flex;
  flex-direction: column;
}

/* 顶部导航 */
.top-nav {
  display: flex;
  justify-content: center;
  gap: 18px;
  padding: 14px 16px;
  height: 50px;
  box-sizing: border-box;
  align-items: center;
  flex-shrink: 0;
}

.tab {
  font-size: 14px;
  color: rgba(255,255,255,0.5);
}

.tab.active {
  color: #fff;
  font-weight: 600;
}

/* 内容区域 */
.content-wrapper {
  flex: 1;
  position: relative;
  overflow: hidden;
  touch-action: pan-y;
  user-select: none;
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.content-wrapper::-webkit-scrollbar {
  display: none;
}

/* 底部导航 */
.bottom-nav {
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding: 14px 16px;
  height: 60px;
  box-sizing: border-box;
  background: #2c2c2e;
  flex-shrink: 0;
}

.nav-label {
  font-size: 13px;
  color: rgba(255,255,255,0.55);
}

.nav-label:first-child {
  color: #fff;
}

.add-box {
  width: 40px;
  height: 28px;
  border: 2px solid #fff;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  color: #fff;
  margin-top: -4px;
}

/* 滑动动画 */
.slide-up-enter-active,
.slide-up-leave-active,
.slide-down-enter-active,
.slide-down-leave-active {
  transition: transform 0.4s cubic-bezier(.4,0,.2,1);
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.slide-up-enter-from {
  transform: translateY(100%);
}

.slide-up-leave-to {
  transform: translateY(-100%);
}

.slide-down-enter-from {
  transform: translateY(-100%);
}

.slide-down-leave-to {
  transform: translateY(100%);
}
</style>