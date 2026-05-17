<template>
  <div class="page-container">
    <!-- 中间内容区 -->
    <div class="content">
      <!-- 英雄区 -->
      <div class="hero-section">
        <img class="hero-bg" src="/13.png" />
        <div class="art-area">
          <div class="art-text">
            <div class="line1">
              <span class="sun-icon">☀️</span>
              <span class="big">清晨</span>
              <span class="small">时光</span>
            </div>
            <div class="line2">送给今天的礼物</div>
          </div>
          <div class="wave-line"></div>
        </div>
        <div class="heart-btn" @click="showDanmakuToggle">
          <img src="/14.png" />
        </div>
        <div class="danmaku" v-if="showDanmaku">早安！今天也要加油哦！</div>
      </div>

      <!-- 模块滑动区域 -->
      <div class="modules-wrapper" ref="modulesRef">
        <Transition :name="slideDirection" mode="out-in">
          <div class="module-page" :key="currentModule">
            <!-- 今日运势 -->
            <div class="module-card fortune" v-if="currentModule === 0"
                :style="{ background: `linear-gradient(135deg, ${fortuneData.luckColor}18 0%, ${fortuneData.luckColor}08 100%)` }">
              <div class="fortune-card-top">
                <img class="fortune-logo" src="/15.png" />
                <div class="fortune-card-title-wrap">
                  <div class="fortune-card-title">今日运势</div>
                  <div class="fortune-card-date">{{ currentDate }}</div>
                </div>
                <div class="fortune-refresh" @click="refreshFortune">
                  <span>换一换</span>
                </div>
              </div>

              <div class="fortune-card-body">
                <div class="fortune-score">
                  <div class="score-label">综合</div>
                  <div class="score-bar">
                    <div class="score-fill" :style="{ width: fortuneData.score + '%' }"></div>
                  </div>
                  <div class="score-num">{{ fortuneData.score }}分</div>
                </div>

                <div class="fortune-meta">
                  <div class="chip">
                    <img class="chip-logo" src="/17.png" />
                    <div class="chip-k">星座</div>
                    <div class="chip-v">{{ fortuneData.zodiac }}</div>
                  </div>
                  <div class="chip">
                    <img class="chip-logo" src="/16.png" />
                    <div class="chip-k">干支</div>
                    <div class="chip-v">{{ fortuneData.ganzhi }}</div>
                  </div>
                  <div class="chip">
                    <img class="chip-logo" src="/18.png" />
                    <div class="chip-k">五行</div>
                    <div class="chip-v">{{ fortuneData.wuxing }}</div>
                  </div>
                </div>

                <div class="fortune-pills">
                  <div class="pill">
                    <div class="pill-k">幸运色</div>
                    <div class="pill-v">
                      <span class="swatch" :style="{ background: fortuneData.luckColor }"></span>
                      {{ fortuneData.luckColorName }}
                    </div>
                  </div>
                  <div class="pill">
                    <div class="pill-k">幸运数</div>
                    <div class="pill-v">{{ fortuneData.luckNumber }}</div>
                  </div>
                  <div class="pill">
                    <div class="pill-k">幸运方向</div>
                    <div class="pill-v">{{ fortuneData.luckDir }}</div>
                  </div>
                  <div class="pill">
                    <div class="pill-k">幸运时段</div>
                    <div class="pill-v">{{ fortuneData.luckTime }}</div>
                  </div>
                </div>

                <div class="fortune-tips">
                  <div class="tip-line">
                    <span class="tip-dot">✦</span>
                    <span>{{ fortuneData.tipA }}</span>
                  </div>
                  <div class="tip-line">
                    <span class="tip-dot">✦</span>
                    <span>{{ fortuneData.tipB }}</span>
                  </div>
                </div>

                <div class="fortune-hint">积极风向：把好运变成可执行的一步</div>
              </div>
            </div>

            <!-- 早餐养生 / 八段锦 -->
            <div class="module-card breakfast" v-else-if="currentModule === 1">
              <div class="fortune-card-top">
                <img class="fortune-logo" src="/19.png" />
                <div class="fortune-card-title-wrap">
                  <div class="fortune-card-title">八段锦</div>
                  <div class="fortune-card-date">今日跟练 · 8-10分钟</div>
                </div>
                <div class="practice-btn" @click="openPracticeModal">
                  <span>我要练</span>
                </div>
              </div>

              <div class="fortune-card-body">
                <div class="video-container">
                  <video ref="videoRef" class="video-player" :src="videoSrc" @ended="onVideoEnd" @play="onVideoPlay" @pause="onVideoPause" playsinline controls></video>
                  <div class="video-overlay" v-if="!isPlaying" @click="toggleVideo">
                    <div class="play-btn">▶</div>
                  </div>
                  <div class="video-progress" v-if="videoDuration > 0">
                    <div class="progress-bar">
                      <div class="progress-fill" :style="{ width: (videoCurrentTime / videoDuration * 100) + '%' }"></div>
                    </div>
                    <div class="video-time">{{ formatTime(videoCurrentTime) }} / {{ formatTime(videoDuration) }}</div>
                  </div>
                </div>
                
                <div class="fortune-hint">八段锦养气以呼吸带动动作，慢而不断，气息绵长</div>
              </div>
            </div>

            <!-- 今日穿搭 -->
            <div class="module-card outfit" v-else>
              <div class="outfit-header">
                <div class="outfit-title">今日穿搭</div>
                <div class="outfit-weather-tag">☀️ 多云转晴 18-26°C</div>
              </div>

              <div class="outfit-showcase">
                <!-- 左侧：风衣展示 -->
                <div class="outfit-main-item">
                  <img src="/20.png" class="coat-img" />
                  <div class="item-label">风衣</div>
                </div>

                <!-- 中间：分层展示 -->
                <div class="outfit-layers">
                  <div class="layer-shirt">
                    <img src="/22.png" class="shirt-img" />
                  </div>
                  <div class="layer-jeans">
                    <img src="/21.png" class="jeans-img" />
                  </div>
                </div>

                <!-- 右侧：包包 -->
                <div class="outfit-accessory">
                  <img src="/23.png" class="bag-img" />
                  <div class="item-label">挎包</div>
                </div>
              </div>

              <div class="outfit-coord">
                <div class="coord-title">今日搭配</div>
                <div class="coord-items">
                  <span class="coord-tag">风衣</span>
                  <span class="coord-arrow">+</span>
                  <span class="coord-tag">衬衫</span>
                  <span class="coord-arrow">+</span>
                  <span class="coord-tag">牛仔裤</span>
                  <span class="coord-arrow">+</span>
                  <span class="coord-tag">挎包</span>
                </div>
              </div>

              <div class="outfit-suggest">
                <div class="suggest-main">轻薄风衣 · 简约出行</div>
                <div class="suggest-sub">早晚御凉，午后单衣，适合户外漫步</div>
              </div>
            </div>
          </div>
        </Transition>

        <!-- 滑动指示器 -->
        <div class="slide-indicators">
          <span class="indicator" :class="{ active: currentModule === 0 }" @click="goToModule(0)"></span>
          <span class="indicator" :class="{ active: currentModule === 1 }" @click="goToModule(1)"></span>
          <span class="indicator" :class="{ active: currentModule === 2 }" @click="goToModule(2)"></span>
        </div>
      </div>

      <!-- 下滑提示 -->
      <div class="swipe-hint" @click="$emit('swipe')">
        <span class="down-arrow">∨</span>
        <span class="hint-text">下滑切换</span>
      </div>
    </div>

    <!-- 弹窗 -->
    <div class="info-modal" v-if="modalVisible" @click.self="closeModal">
      <div class="info-popup">
        <div class="popup-header">
          <span class="popup-title">{{ modalData.title }}</span>
          <span class="popup-close" @click="closeModal">×</span>
        </div>
        <div class="popup-content">
          <img v-if="modalData.icon.endsWith('.png')" class="popup-icon-img" :src="modalData.icon" />
          <div v-else class="popup-icon">{{ modalData.icon }}</div>
          <div class="popup-desc">{{ modalData.desc }}</div>
        </div>
        <button class="popup-btn" @click="closeModal">我知道了</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useSwipe } from '@vueuse/core'

const showDanmaku = ref(false)
const modalVisible = ref(false)
const modalData = ref({ title: '', icon: '', desc: '' })
const currentModule = ref(0)
const slideDirection = ref('slide-left')
const modulesRef = ref(null)

const isPlaying = ref(false)
const videoSrc = '/xiaoyunque.mp4'
const videoRef = ref(null)
const videoDuration = ref(0)
const videoCurrentTime = ref(0)
let videoTimer = null

const toggleVideo = () => {
  if (!videoRef.value) return
  if (isPlaying.value) {
    videoRef.value.pause()
    clearInterval(videoTimer)
  } else {
    videoRef.value.play()
    videoTimer = setInterval(() => {
      if (videoRef.value) {
        videoCurrentTime.value = videoRef.value.currentTime
      }
    }, 1000)
  }
  isPlaying.value = !isPlaying.value
}

const restartVideo = () => {
  if (videoRef.value) {
    videoRef.value.currentTime = 0
    videoRef.value.play()
    isPlaying.value = true
    clearInterval(videoTimer)
    videoTimer = setInterval(() => {
      if (videoRef.value) {
        videoCurrentTime.value = videoRef.value.currentTime
      }
    }, 1000)
  }
}

const onVideoEnd = () => {
  isPlaying.value = false
  clearInterval(videoTimer)
}

const onVideoPlay = () => {
  // Video playing
}

const onVideoPause = () => {
  clearInterval(videoTimer)
}

const formatTime = (seconds) => {
  const m = Math.floor(seconds / 60)
  const s = Math.floor(seconds % 60)
  return `${m.toString().padStart(2, '0')}:${s.toString().padStart(2, '0')}`
}

const openVideoModal = () => {
  modalData.value = {
    title: '八段锦教程',
    icon: '🧘',
    desc: '预备式：两足开立，与肩同宽，双手抱拳于腹前...\n\n第一式：双手托天理三焦\n第二式：左右开弓似射雕\n第三式：调理脾胃须单举\n第四式：五劳七伤往后瞧\n第五式：摇头摆尾去心火\n第六式：双手攀足固肾腰\n第七式：攒拳怒目增气力\n第八式：背后七颠百病消\n\n收式：双手合抱于丹田，呼吸均匀。'
  }
  modalVisible.value = true
}

const openPracticeModal = () => {
  modalData.value = {
    title: '八段锦攻略教程',
    icon: '19.png',
    desc: '最佳时间：清晨空腹或晚饭后2小时\n\n准备：穿宽松衣物，室温适宜\n\n呼吸要点：\n• 用鼻呼吸，不用嘴\n• 动作轻柔缓慢\n• 保持自然呼吸节奏\n\n注意事项：\n• 膝盖有伤者降低幅度\n• 高血压患者避免憋气\n• 女性生理期减少运动量\n\n功效：\n• 调理气血运行\n• 增强心肺功能\n• 舒缓颈肩腰背不适'
  }
  modalVisible.value = true
}

const currentDate = new Date().toLocaleDateString('zh-CN', { month: 'long', day: 'numeric', weekday: 'long' })

const fortuneData = ref({
  score: 85,
  zodiac: '天秤座',
  ganzhi: '壬寅年四月廿三',
  wuxing: '金',
  luckColor: '#FFD700',
  luckColorName: '金色',
  luckNumber: 7,
  luckDir: '东南方',
  luckTime: '9-11点',
  tipA: '今日宜主动表达想法，容易获得认可',
  tipB: '注意保护眼睛，避免长时间用眼'
})

const { direction } = useSwipe(modulesRef, {
  onSwipeEnd(e, dir) {
    if (dir === 'left' && currentModule.value < 2) {
      slideDirection.value = 'slide-left'
      currentModule.value++
    }
    if (dir === 'right' && currentModule.value > 0) {
      slideDirection.value = 'slide-right'
      currentModule.value--
    }
  }
})

const showDanmakuToggle = () => {
  showDanmaku.value = true
  setTimeout(() => {
    showDanmaku.value = false
  }, 3000)
}

const refreshFortune = () => {
  const scores = [78, 82, 85, 88, 92]
  const zodiacs = ['天秤座', '处女座', '射手座', '双子座', '巨蟹座']
  const wuxing = ['金', '木', '水', '火', '土']
  const dirs = ['东南方', '正北方', '西南方', '正东方']
  const times = ['9-11点', '13-15点', '17-19点', '21-23点']
  const tips = [
    '今日适合团队协作，容易碰撞出灵感火花',
    '注意控制饮食，避免暴饮暴食',
    '有贵人运，可以主动联系许久未联系的朋友',
    '适合处理文书工作或学习新知识'
  ]

  fortuneData.value = {
    score: scores[Math.floor(Math.random() * scores.length)],
    zodiac: zodiacs[Math.floor(Math.random() * zodiacs.length)],
    ganzhi: '壬寅年四月廿三',
    wuxing: wuxing[Math.floor(Math.random() * wuxing.length)],
    luckColor: '#FFD700',
    luckColorName: '金色',
    luckNumber: Math.floor(Math.random() * 9) + 1,
    luckDir: dirs[Math.floor(Math.random() * dirs.length)],
    luckTime: times[Math.floor(Math.random() * times.length)],
    tipA: tips[Math.floor(Math.random() * tips.length)],
    tipB: tips[Math.floor(Math.random() * tips.length)]
  }
}

const modalContents = {
  fortune: {
    title: '今日运势',
    icon: '🔮',
    desc: '今日运势极佳！适合制定新计划、尝试新事物。工作上可能有机会展现才华，感情上人际运势上涨。单打独斗不如借力使力，多和他人交流会有意外收获哦！'
  },
  breakfast: {
    title: '养胃早餐推荐',
    icon: '🥣',
    desc: '推荐：小米粥+鸡蛋+小菜\n\n早起空腹喝一杯温水，唤醒肠胃。早餐以温热软烂为主，避免辛辣油腻。养胃从每一顿早餐开始！'
  },
  outfit: {
    title: '今日穿搭建议',
    icon: '👔',
    desc: '今日天气：多云转晴，18-26°C\n\n建议：轻薄长袖或短袖+薄外套，方便早晚增减。颜色可选浅色系，清新自然。适合户外活动，记得带水杯！'
  }
}

const openModal = (type) => {
  modalData.value = modalContents[type]
  modalVisible.value = true
}

const goToModule = (index) => {
  if (index > currentModule.value) {
    slideDirection.value = 'slide-left'
  } else {
    slideDirection.value = 'slide-right'
  }
  currentModule.value = index
}

const closeModal = () => {
  modalVisible.value = false
}
</script>

<style scoped>
.page-container {
  height: 100%;
  display: flex;
  flex-direction: column;
  font-family: -apple-system, BlinkMacSystemFont, "PingFang SC", sans-serif;
  background: #000;
  overflow: hidden;
}

.content {
  flex: 1;
  overflow-y: auto;
  overflow-x: hidden;
  display: flex;
  flex-direction: column;
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.content::-webkit-scrollbar {
  display: none;
}

/* 英雄区 */
.hero-section {
  height: 28%;
  position: relative;
  overflow: hidden;
  flex-shrink: 0;
}

.hero-bg {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center 35%;
}

.art-area {
  position: absolute;
  left: 16px;
  top: 55%;
  transform: translateY(-50%);
}

.art-text {
  font-family: 'Ma Shan Zheng', cursive;
}

.line1 {
  display: flex;
  align-items: center;
  gap: 4px;
}

.sun-icon {
  font-size: 24px;
  filter: drop-shadow(0 0 6px rgba(255,200,50,0.6));
}

.big {
  font-size: 30px;
  color: #ffffff;
  text-shadow: 0 0 6px rgba(217, 239, 93, 0.767);
}

.small {
  font-size: 24px;
  color: #ffffff;
  text-shadow: 0 0 6px rgba(255,213,79,0.6);
}

.line2 {
  font-size: 20px;
  color: rgba(255,213,79,0.8);
  margin-top: 2px;
}

.wave-line {
  width: 60px;
  height: 2px;
  margin-top: 6px;
  background: linear-gradient(to right, rgba(255,213,79,0.7), transparent);
  border-radius: 1px;
}

.heart-btn {
  position: absolute;
  right: 16px;
  bottom: 16px;
  width: 52px;
  height: 52px;
  cursor: pointer;
  z-index: 5;
}

.heart-btn img {
  width: 100%;
  height: 100%;
}

.danmaku {
  position: absolute;
  left: 50%;
  bottom: 20px;
  transform: translateX(-50%);
  background: rgba(255,213,79,0.9);
  color: #333;
  padding: 6px 16px;
  border-radius: 16px;
  font-size: 13px;
  white-space: nowrap;
  animation: fadeInOut 3s forwards;
  z-index: 10;
}

@keyframes fadeInOut {
  0% { opacity: 0; transform: translateX(-50%) translateY(10px); }
  15% { opacity: 1; transform: translateX(-50%) translateY(0); }
  70% { opacity: 1; }
  100% { opacity: 0; transform: translateX(-50%) translateY(-10px); }
}

/* 模块滑动区域 */
.modules-wrapper {
  flex: 1;
  position: relative;
  overflow: hidden;
  min-height: 0;
  background: #0b1f17;
}

.module-page {
  position: absolute;
  inset: 0;
  padding: 16px;
  display: flex;
  flex-direction: column;
}

.module-card {
  flex: 1;
  background: linear-gradient(180deg, #1a3a2a 0%, #0d1f18 100%);
  border-radius: 16px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 1px solid rgba(255,213,79,0.15);
}

.module-icon {
  font-size: 40px;
  margin-bottom: 8px;
}

.module-title {
  font-size: 18px;
  font-weight: 600;
  color: #ffd54f;
  margin-bottom: 16px;
}

.module-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
  width: 100%;
}

/* 运势卡片 */
.fortune-stars {
  font-size: 24px;
  color: #ffd54f;
  letter-spacing: 4px;
}

.fortune-desc {
  font-size: 14px;
  color: rgba(255,255,255,0.8);
  text-align: center;
  line-height: 1.5;
}

.fortune-detail {
  display: flex;
  gap: 24px;
  background: rgba(255,255,255,0.05);
  border-radius: 12px;
  padding: 12px 20px;
  width: 100%;
  justify-content: center;
}

.fortune-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
}

.fortune-label {
  font-size: 12px;
  color: rgba(255,255,255,0.5);
}

.fortune-value {
  font-size: 14px;
  color: #ffd54f;
}

.fortune-tip {
  font-size: 12px;
  color: rgba(255,213,79,0.7);
  background: rgba(255,213,79,0.1);
  padding: 10px 16px;
  border-radius: 8px;
  text-align: center;
  line-height: 1.4;
}

/* 运势模块 */
.module-card.fortune {
  padding: 16px;
  gap: 12px;
}

.fortune-card-top {
  display: flex;
  align-items: center;
  gap: 12px;
}

.fortune-card-title-wrap {
  flex: 1;
}

.fortune-logo {
  width: 52px;
  height: 52px;
  filter: brightness(1.15);
}

.fortune-card-title-wrap {
  flex: 1;
}

.fortune-card-title {
  font-size: 16px;
  font-weight: 600;
  color: #fff;
}

.fortune-card-date {
  font-size: 11px;
  color: rgba(255,255,255,0.5);
}

.fortune-refresh {
  padding: 6px 12px;
  background: rgba(255,213,79,0.15);
  border-radius: 14px;
  font-size: 12px;
  color: #ffd54f;
  cursor: pointer;
}

.fortune-card-body {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.fortune-score {
  display: flex;
  align-items: center;
  gap: 10px;
}

.score-label {
  font-size: 12px;
  color: rgba(255,255,255,0.7);
  width: 30px;
}

.score-bar {
  flex: 1;
  height: 6px;
  background: rgba(255,255,255,0.1);
  border-radius: 3px;
  overflow: hidden;
}

.score-fill {
  height: 100%;
  background: linear-gradient(90deg, #ffd54f, #ffb300);
  border-radius: 3px;
  transition: width 0.3s;
}

.score-num {
  font-size: 12px;
  color: #ffd54f;
  font-weight: 600;
  width: 40px;
  text-align: right;
}

.fortune-meta {
  display: flex;
  gap: 8px;
}

.chip {
  flex: none;
  width: 90px;
  background: rgba(255,255,255,0.05);
  border-radius: 10px;
  padding: 10px 8px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2px;
}

.chip-logo {
  width: 36px;
  height: 36px;
  margin-bottom: 2px;
  filter: brightness(1.2);
}

.chip-k {
  font-size: 10px;
  color: rgba(255,255,255,0.4);
}

.chip-v {
  font-size: 11px;
  color: #fff;
  font-weight: 500;
}

.fortune-pills {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
}

.pill {
  background: rgba(255,255,255,0.05);
  border-radius: 8px;
  padding: 8px 10px;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.pill-k {
  font-size: 10px;
  color: rgba(255,255,255,0.4);
}

.pill-v {
  font-size: 12px;
  color: #fff;
  display: flex;
  align-items: center;
  gap: 6px;
}

.swatch {
  width: 14px;
  height: 14px;
  border-radius: 50%;
}

.fortune-tips {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.tip-line {
  font-size: 12px;
  color: rgba(255,255,255,0.7);
  display: flex;
  align-items: center;
  gap: 8px;
}

.tip-dot {
  color: #ffd54f;
}

.fortune-hint {
  font-size: 11px;
  color: rgba(255,213,79,0.6);
  text-align: center;
}

/* 早餐卡片 */
.breakfast-main {
  font-size: 20px;
  font-weight: 600;
  color: #fff;
}

.breakfast-note {
  font-size: 13px;
  color: rgba(255,255,255,0.6);
}

.breakfast-tips {
  display: flex;
  flex-direction: column;
  gap: 8px;
  background: rgba(255,255,255,0.05);
  border-radius: 12px;
  padding: 14px 16px;
  width: 100%;
}

.tip-item {
  font-size: 13px;
  color: rgba(255,255,255,0.8);
}

.breakfast-care {
  font-size: 14px;
  color: rgba(255,213,79,0.8);
  background: rgba(255,213,79,0.1);
  padding: 10px 16px;
  border-radius: 8px;
}

/* 穿搭卡片 */

/* 今日穿搭模块 */
.outfit-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  margin-bottom: 16px;
}

.outfit-title {
  font-size: 16px;
  font-weight: 600;
  color: #fff;
}

.outfit-weather-tag {
  font-size: 12px;
  color: rgba(255,255,255,0.7);
  background: rgba(255,255,255,0.08);
  padding: 4px 10px;
  border-radius: 12px;
}

.outfit-showcase {
  display: flex;
  align-items: flex-end;
  justify-content: center;
  gap: 12px;
  height: 140px;
  margin-bottom: 16px;
}

.outfit-main-item {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.coat-img {
  width: 80px;
  height: auto;
  object-fit: contain;
}

.outfit-layers {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
}

.layer-shirt .shirt-img {
  width: 70px;
  height: auto;
  object-fit: contain;
  margin-top: 40px;
}

.layer-jeans .jeans-img {
  width: 60px;
  height: auto;
  object-fit: contain;
}

.outfit-accessory {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.bag-img {
  width: 45px;
  height: auto;
  object-fit: contain;
}

.item-label {
  font-size: 11px;
  color: rgba(255,255,255,0.6);
  margin-top: 4px;
}

.outfit-coord {
  background: rgba(255,255,255,0.05);
  border-radius: 12px;
  padding: 12px 16px;
  margin-bottom: 12px;
}

.coord-title {
  font-size: 11px;
  color: rgba(255,255,255,0.5);
  margin-bottom: 8px;
  text-align: center;
}

.coord-items {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  flex-wrap: wrap;
}

.coord-tag {
  font-size: 12px;
  color: rgba(255,255,255,0.85);
  background: rgba(255,213,79,0.15);
  padding: 4px 10px;
  border-radius: 12px;
}

.coord-arrow {
  font-size: 12px;
  color: rgba(255,255,255,0.4);
}

.outfit-suggest {
  text-align: center;
  padding: 8px 0;
}

.suggest-main {
  font-size: 15px;
  font-weight: 500;
  color: #fff;
}

.suggest-sub {
  font-size: 12px;
  color: rgba(255,255,255,0.6);
  margin-top: 4px;
}

/* 滑动指示器 */
.slide-indicators {
  position: absolute;
  bottom: 60px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 8px;
  z-index: 10;
}

.indicator {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: rgba(255,255,255,0.3);
}

.indicator.active {
  background: #ffd54f;
  width: 18px;
  border-radius: 3px;
}

/* 下滑提示 */
.swipe-hint {
  position: absolute;
  bottom: 16px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  z-index: 100;
}

.down-arrow {
  font-size: 12px;
  color: rgba(255,255,255,0.5);
  animation: bounceDown 1.5s infinite;
}

.hint-text {
  font-size: 11px;
  color: rgba(255,255,255,0.4);
}

@keyframes bounceDown {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(3px); }
}

/* 滑动动画 */
.slide-left-enter-active,
.slide-left-leave-active,
.slide-right-enter-active,
.slide-right-leave-active {
  transition: transform 0.3s ease;
  position: absolute;
  inset: 0;
  padding: 16px;
}

.slide-left-enter-from {
  transform: translateX(100%);
}

.slide-left-leave-to {
  transform: translateX(-100%);
}

.slide-right-enter-from {
  transform: translateX(-100%);
}

.slide-right-leave-to {
  transform: translateX(100%);
}

/* 弹窗 */
.info-modal {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 200;
}

.info-popup {
  width: 300px;
  background: #1a1a2e;
  border-radius: 16px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
}

.popup-header {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.popup-title {
  font-size: 14px;
  font-weight: 600;
  color: #fff;
}

.popup-close {
  font-size: 24px;
  color: rgba(255,255,255,0.5);
  cursor: pointer;
  line-height: 1;
}

.popup-content {
  text-align: center;
}

.popup-icon {
  font-size: 48px;
  margin-bottom: 12px;
}

.popup-icon-img {
  width: 60px;
  height: 60px;
  margin-bottom: 12px;
}

.popup-desc {
  font-size: 12px;
  color: rgba(255,255,255,0.7);
  line-height: 1.6;
  white-space: pre-line;
}

.popup-btn {
  padding: 8px 24px;
  border-radius: 20px;
  border: none;
  font-size: 13px;
  cursor: pointer;
  background: #ffd54f;
  color: #333;
  font-weight: 500;
}

/* 八段锦模块 */
.breakfast-header {
  text-align: center;
  margin-bottom: 12px;
}

.breakfast-title-row {
  display: flex;
  align-items: center;
  gap: 12px;
}

.title-icon {
  width: 75px;
  height: 75px;
}

.breakfast-title {
  font-size: 20px;
  font-weight: 600;
  color: #fff;
}

.breakfast-badge {
  font-size: 10px;
  background: rgba(255,213,79,0.2);
  color: #ffd54f;
  padding: 2px 8px;
  border-radius: 10px;
}

.practice-btn {
  padding: 6px 16px;
  background: rgba(255,213,79,0.15);
  border: 1px solid rgba(255,213,79,0.3);
  border-radius: 16px;
  font-size: 12px;
  color: #ffd54f;
  cursor: pointer;
  display: block;
  margin: 10px auto 0;
}

.breakfast-subtitle {
  font-size: 12px;
  color: rgba(255,255,255,0.6);
  margin-top: 4px;
}

.video-container {
  position: relative;
  width: 100%;
  background: #000;
  border-radius: 12px;
  overflow: hidden;
  margin-bottom: 12px;
}

.video-player {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.video-overlay {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0,0,0,0.3);
  cursor: pointer;
}

.play-btn {
  width: 50px;
  height: 50px;
  background: rgba(255,213,79,0.9);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  color: #333;
}

.video-progress {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 8px;
  background: linear-gradient(to top, rgba(0,0,0,0.6), transparent);
}

.progress-bar {
  height: 3px;
  background: rgba(255,255,255,0.3);
  border-radius: 2px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: #ffd54f;
  transition: width 0.3s;
}

.video-time {
  font-size: 10px;
  color: rgba(255,255,255,0.7);
  margin-top: 4px;
  text-align: center;
}

.breakfast-desc {
  font-size: 12px;
  color: rgba(255,255,255,0.6);
  text-align: center;
  line-height: 1.5;
  margin-bottom: 12px;
}

.breakfast-controls {
  width: 100%;
}

.ctrl-btn-row {
  display: flex;
  gap: 8px;
}

.ctrl-btn {
  flex: 1;
  padding: 10px 12px;
  border-radius: 20px;
  border: none;
  font-size: 12px;
  font-weight: 500;
  cursor: pointer;
  background: #ffd54f;
  color: #333;
}

.ctrl-btn.secondary {
  background: rgba(255,255,255,0.1);
  color: rgba(255,255,255,0.8);
}
</style>