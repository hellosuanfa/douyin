<template>
  <div class="page-container">
    <!-- 中间内容区 -->
    <div class="content">
      <!-- 英雄区 -->
      <div class="hero-section">
        <img class="hero-bg" src="/3.png" />
        <div class="art-area">
          <div class="art-text">
            <div class="line1">
              <span class="big">深夜</span>
              <span class="small">时光</span>
              <div class="moon-stars">
                <svg class="moon" viewBox="0 0 40 40">
                  <path d="M20 5 C12 5 6 12 6 20 C6 28 12 35 20 35 C14 33 9 27 9 20 C9 13 14 7 20 5" fill="#ffefb8"/>
                </svg>
                <svg class="star" viewBox="0 0 20 20">
                  <path d="M10 0 L12 8 L20 10 L12 12 L10 20 L8 12 L0 10 L8 8 Z" fill="#ffefb8"/>
                </svg>
              </div>
            </div>
            <div class="line2">属于自己的温柔</div>
          </div>
          <div class="wave-line"></div>
        </div>
        <div class="heart-btn" @click="showDanmakuToggle">
          <img src="/heart.png" />
        </div>
        <div class="danmaku" v-if="showDanmaku">爱你老己，明天见！</div>
      </div>

      <!-- 模块区域 -->
      <div class="modules">
        <!-- 恢复练习 -->
        <div class="section">
          <div class="section-header">
            <span class="section-title">恢复练习</span>
            <span class="section-more">全部 ></span>
          </div>
          <div class="exercise-row">
            <div class="exercise-item" @click="openExercise('stretch')">
              <img class="ex-icon" src="/ex1.png" />
              <div class="exercise-name">深度拉伸</div>
              <div class="exercise-time">8分钟</div>
              <div class="exercise-status not-start">▶</div>
            </div>
            <div class="exercise-item" @click="openExercise('breath')">
              <img class="ex-icon" src="/ex2.png" />
              <div class="exercise-name">呼吸放松</div>
              <div class="exercise-time">6分钟</div>
              <div class="exercise-status done">✓</div>
            </div>
            <div class="exercise-item" @click="openExercise('neck')">
              <img class="ex-icon" src="/ex3.png" />
              <div class="exercise-name">肩颈舒缓</div>
              <div class="exercise-time">7分钟</div>
              <div class="exercise-status progress">◐</div>
            </div>
          </div>
        </div>

        <!-- 助眠音乐 -->
        <div class="section music-section">
          <div class="section-header">
            <span class="section-title">助眠音乐</span>
            <span class="section-more">更多 ></span>
          </div>
          <div class="music-row">
            <div class="music-cover-box" @click="togglePlay">
              <img class="music-img" src="/3.png" />
              <div class="music-play" :class="{ playing: isPlaying }">
                {{ isPlaying ? '❚❚' : '▶' }}
              </div>
            </div>
            <div class="music-info">
              <div class="music-name">你离开的真相</div>
              <div class="music-type">钢琴 · 高至豪</div>
              <div class="music-bar">
                <div class="music-progress" :style="{ width: progressPercent + '%' }"></div>
              </div>
              <div class="music-time">{{ formatTime(currentTime) }} / {{ formatTime(duration) }}</div>
            </div>
            <div class="music-wave" :class="{ active: isPlaying }">
              <span></span><span></span><span></span><span></span>
            </div>
          </div>
        </div>

        <!-- 安心食物 -->
        <div class="section food-section">
          <div class="section-header">
            <span class="section-title">安心食物</span>
            <span class="section-more">全部食谱 ></span>
          </div>
          <div class="food-card">
            <div class="food-img-box">
              <img src="/4.jpg" />
              <div class="food-time">15min</div>
            </div>
            <div class="food-content">
              <div class="food-name">红枣小米粥</div>
              <div class="food-desc">暖胃安神，助眠好选择</div>
              <div class="food-ingredients">
                <img class="ing-icon" src="/xiaomi.png" />
                <img class="ing-icon" src="/hongzao.png" />
                <img class="ing-icon" src="/gouqipng.png" />
                <img class="ing-icon" src="/bingtang.png" />
              </div>
              <div class="ing-labels">
                <span class="ing-label">小米</span>
                <span class="ing-label">红枣</span>
                <span class="ing-label">枸杞</span>
                <span class="ing-label">冰糖</span>
              </div>
            </div>
            <div class="food-action-btn" @click="openFoodTutorial">
              <span class="action-text">查看</span>
              <span class="action-text">教程</span>
            </div>
          </div>
        </div>
      </div>

      <!-- 底部按钮 -->
      <div class="bottom-btns">
        <div class="btn-small">不感兴趣</div>
        <div class="btn-large">更多养生</div>
      </div>
      <!-- 上滑提示 -->
      <div class="swipe-hint" @click="$emit('swipe')">
        <span class="up-arrow">∧</span>
        <span class="hint-text">上滑切换</span>
      </div>
    </div>

    <!-- 运动弹窗 -->
    <div class="exercise-modal" v-if="exerciseModal" @click.self="closeExercise">
      <div class="exercise-popup">
        <div class="exercise-header">
          <span class="exercise-title">{{ currentExercise?.title }}</span>
          <span class="exercise-close" @click="closeExercise">×</span>
        </div>
        <div class="exercise-time-display">{{ formatCountdown(countdown) }}</div>
        <div class="exercise-ring">
          <svg viewBox="0 0 100 100">
            <circle class="ring-bg" cx="50" cy="50" r="45" />
            <circle class="ring-progress" cx="50" cy="50" r="45"
              :stroke-dasharray="283"
              :stroke-dashoffset="283 - (283 * countdown / totalTime)" />
          </svg>
        </div>
        <div class="exercise-steps">
          <div class="step-title">讲解：</div>
          <div class="step-item" v-for="(step, i) in currentExercise?.steps" :key="i">
            {{ i + 1 }}. {{ step }}
          </div>
        </div>
        <div class="exercise-controls">
          <button class="ctrl-btn" @click="toggleCountdown">
            {{ isCounting ? '暂停' : '开始' }}
          </button>
          <button class="ctrl-btn reset" @click="resetCountdown">重置</button>
        </div>
      </div>
    </div>

    <!-- 食物教程弹窗 -->
    <div class="food-modal" v-if="foodTutorialModal" @click.self="closeFoodTutorial">
      <div class="food-popup">
        <div class="food-header">
          <span class="food-title">红枣小米粥 · 教程</span>
          <span class="food-close" @click="closeFoodTutorial">×</span>
        </div>
        <div class="food-main-img">
          <img src="/4.jpg" />
        </div>
        <div class="food-ingredients-row">
          <div class="ing-item">
            <img src="/xiaomi.png" />
            <span>小米</span>
          </div>
          <div class="ing-item">
            <img src="/hongzao.png" />
            <span>红枣</span>
          </div>
          <div class="ing-item">
            <img src="/gouqipng.png" />
            <span>枸杞</span>
          </div>
          <div class="ing-item">
            <img src="/bingtang.png" />
            <span>冰糖</span>
          </div>
        </div>
        <div class="food-steps">
          <div class="food-step">
            <div class="step-num">1</div>
            <div class="step-content">
              <div class="step-name">准备食材</div>
              <div class="step-desc">小米 50g洗净，红枣 5颗去核，枸杞 10g，冰糖适量</div>
            </div>
          </div>
          <div class="food-step">
            <div class="step-num">2</div>
            <div class="step-content">
              <div class="step-name">煮粥</div>
              <div class="step-desc">小米加水没过食材，大火煮开后转小火熬 15 分钟</div>
            </div>
          </div>
          <div class="food-step">
            <div class="step-num">3</div>
            <div class="step-content">
              <div class="step-name">加入红枣</div>
              <div class="step-desc">红枣去核后加入，继续熬煮 10 分钟至软烂</div>
            </div>
          </div>
          <div class="food-step">
            <div class="step-num">4</div>
            <div class="step-content">
              <div class="step-name">最后调味</div>
              <div class="step-desc">加入枸杞和冰糖，再煮 5 分钟即可享用</div>
            </div>
          </div>
        </div>
        <button class="food-done-btn" @click="closeFoodTutorial">学会了</button>
      </div>
    </div>
</div>
</template>

<script setup>
import { ref, computed, onUnmounted } from 'vue'

const isPlaying = ref(false)
const showDanmaku = ref(false)
const audio = ref(null)
const currentTime = ref(0)
const duration = ref(0)

const exerciseModal = ref(false)
const foodTutorialModal = ref(false)
const isCounting = ref(false)
const countdown = ref(0)
const totalTime = ref(480)
let timer = null

const exercises = {
  stretch: {
    title: '深度拉伸 · 8 分钟',
    duration: 480,
    steps: [
      '腿后侧拉伸：左右各 40 秒，保持呼吸。',
      '髋部打开：左右各 40 秒，别顶膝。',
      '背部放松：猫牛式 60 秒，动作小而稳。',
      '最后 1 分钟：躺平伸展，放松下颌和肩。'
    ]
  },
  breath: {
    title: '呼吸放松 · 6 分钟',
    duration: 360,
    steps: [
      '热身：自然呼吸 1 分钟，放松身体。',
      '腹式呼吸：吸气 4 秒，屏息 4 秒，呼气 6 秒，重复 8 次。',
      '478 呼吸法：吸气 4 秒，屏息 7 秒，呼气 8 秒，重复 6 次。',
      '放松收尾：自然呼吸 2 分钟，感受身体放松。'
    ]
  },
  neck: {
    title: '肩颈舒缓 · 7 分钟',
    duration: 420,
    steps: [
      '颈部热身：头部前后左右各 15 秒，动作轻柔。',
      '颈部环绕：顺时针、逆时针各 30 秒，速度缓慢。',
      '肩部上提：吸气上提，呼气放下，重复 20 次。',
      '肩部拉伸：右手扶左耳，头向右倾，换边各 40 秒。',
      '斜方肌拉伸：左手扶右耳，右手向后拉，交叉各 30 秒。',
      '放松按摩：用手掌轻揉肩颈 1 分钟。'
    ]
  }
}

const currentExercise = computed(() => exercises[currentExerciseKey.value])
const currentExerciseKey = ref('stretch')

const openExercise = (key) => {
  currentExerciseKey.value = key
  totalTime.value = exercises[key].duration
  countdown.value = exercises[key].duration
  isCounting.value = false
  exerciseModal.value = true
}

const closeExercise = () => {
  exerciseModal.value = false
  isCounting.value = false
}

const openFoodTutorial = () => {
  foodTutorialModal.value = true
}

const closeFoodTutorial = () => {
  foodTutorialModal.value = false
}

const toggleCountdown = () => {
  if (isCounting.value) {
    clearInterval(timer)
    isCounting.value = false
  } else {
    timer = setInterval(() => {
      if (countdown.value > 0) {
        countdown.value--
      } else {
        clearInterval(timer)
        isCounting.value = false
      }
    }, 1000)
    isCounting.value = true
  }
}

const resetCountdown = () => {
  clearInterval(timer)
  countdown.value = totalTime.value
  isCounting.value = false
}

const formatCountdown = (seconds) => {
  const m = Math.floor(seconds / 60)
  const s = seconds % 60
  return `${m.toString().padStart(2, '0')}:${s.toString().padStart(2, '0')}`
}

const togglePlay = () => {
  if (!audio.value) {
    audio.value = new Audio('/music.mp3')
    audio.value.loop = true
    audio.value.addEventListener('loadedmetadata', () => {
      duration.value = audio.value.duration
    })
    audio.value.addEventListener('timeupdate', () => {
      currentTime.value = audio.value.currentTime
    })
  }
  if (isPlaying.value) {
    audio.value.pause()
  } else {
    audio.value.play()
  }
  isPlaying.value = !isPlaying.value
}

const showDanmakuToggle = () => {
  showDanmaku.value = true
  setTimeout(() => {
    showDanmaku.value = false
  }, 3000)
}

const formatTime = (seconds) => {
  const m = Math.floor(seconds / 60)
  const s = Math.floor(seconds % 60)
  return `${m.toString().padStart(2, '0')}:${s.toString().padStart(2, '0')}`
}

const progressPercent = computed(() => {
  if (duration.value === 0) return 0
  return Math.min((currentTime.value / duration.value) * 100, 100)
})

onUnmounted(() => {
  if (audio.value) {
    audio.value.pause()
    audio.value = null
  }
})
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

.big {
  font-size: 28px;
  color: #ffefb8;
  text-shadow: 0 0 6px rgba(255,239,184,0.6);
}

.small {
  font-size: 22px;
  color: #ffefb8;
  text-shadow: 0 0 6px rgba(255,239,184,0.6);
}

.moon-stars {
  display: flex;
  align-items: center;
  gap: 2px;
  margin-left: 2px;
}

.moon {
  width: 20px;
  height: 20px;
  filter: drop-shadow(0 0 4px rgba(255,239,184,0.7));
}

.star {
  width: 10px;
  height: 10px;
  filter: drop-shadow(0 0 3px rgba(255,239,184,0.6));
}

.line2 {
  font-size: 13px;
  color: rgba(255,239,184,0.8);
  margin-top: 2px;
}

.wave-line {
  width: 60px;
  height: 2px;
  margin-top: 6px;
  background: linear-gradient(to right, rgba(255,239,184,0.7), transparent);
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
  background: rgba(255,239,184,0.9);
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

/* 模块区域 */
.modules {
  padding: 8px 12px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.section {
  background: rgba(255,255,255,0.03);
  border-radius: 10px;
  padding: 8px 10px;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 6px;
}

.section-title {
  font-size: 14px;
  font-weight: 600;
  color: rgba(255,255,255,0.9);
}

.section-more {
  font-size: 10px;
  color: rgba(255,255,255,0.35);
}

/* 恢复练习 */
.exercise-row {
  display: flex;
  gap: 6px;
}

.exercise-item {
  flex: 1;
  background: rgba(255,255,255,0.04);
  border-radius: 8px;
  padding: 8px 6px;
  text-align: center;
  cursor: pointer;
}

.ex-icon {
  width: 28px;
  height: 28px;
  margin-bottom: 4px;
}

.exercise-name {
  font-size: 10px;
  color: #fff;
  font-weight: 500;
}

.exercise-time {
  font-size: 9px;
  color: rgba(255,255,255,0.4);
  margin-top: 2px;
}

.exercise-status {
  font-size: 10px;
  margin-top: 4px;
}

.exercise-status.not-start { color: #ffefb8; }
.exercise-status.done { color: #4ade80; }
.exercise-status.progress { color: #ffefb8; }

/* 助眠音乐 */
.music-section {
  flex-shrink: 0;
}

.music-row {
  display: flex;
  align-items: center;
  gap: 8px;
}

.music-cover-box {
  width: 48px;
  height: 48px;
  position: relative;
}

.music-img {
  width: 100%;
  height: 100%;
  border-radius: 8px;
  object-fit: cover;
}

.music-play {
  position: absolute;
  inset: 0;
  background: rgba(0,0,0,0.4);
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  color: #ffefb8;
  padding-left: 2px;
  cursor: pointer;
}

.music-play.playing {
  background: rgba(255,239,184,0.3);
}

.music-info {
  flex: 1;
}

.music-name {
  font-size: 12px;
  font-weight: 600;
  color: #fff;
}

.music-type {
  font-size: 10px;
  color: rgba(255,255,255,0.4);
  margin-top: 1px;
}

.music-bar {
  height: 3px;
  background: rgba(255,255,255,0.1);
  border-radius: 2px;
  margin-top: 4px;
  overflow: hidden;
}

.music-progress {
  height: 100%;
  background: #ffefb8;
  border-radius: 2px;
}

.music-time {
  font-size: 9px;
  color: rgba(255,255,255,0.4);
  margin-top: 2px;
}

.music-wave {
  display: flex;
  align-items: center;
  gap: 2px;
  height: 18px;
}

.music-wave span {
  width: 2px;
  background: rgba(255,239,184,0.4);
  border-radius: 1px;
}

.music-wave.active span {
  background: linear-gradient(to top, #ffefb8, #ffd54f);
  animation: wave 1s infinite ease-in-out;
}

.music-wave.active span:nth-child(1) { animation-delay: 0s; }
.music-wave.active span:nth-child(2) { animation-delay: 0.15s; height: 12px; }
.music-wave.active span:nth-child(3) { animation-delay: 0.3s; }
.music-wave.active span:nth-child(4) { animation-delay: 0.45s; height: 14px; }

@keyframes wave {
  0%, 100% { opacity: 0.4; }
  50% { opacity: 1; }
}

/* 安心食物 */
.food-section {
  flex-shrink: 0;
}

.food-card {
  display: flex;
  gap: 10px;
  background: rgba(255,255,255,0.04);
  border-radius: 10px;
  padding: 8px;
}

.food-img-box {
  width: 80px;
  height: 80px;
  position: relative;
  border-radius: 8px;
  overflow: hidden;
  flex-shrink: 0;
}

.food-img-box img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.food-time {
  position: absolute;
  top: 4px;
  right: 4px;
  background: rgba(255,239,184,0.9);
  border-radius: 6px;
  padding: 2px 6px;
  font-size: 9px;
  color: #333;
  font-weight: 600;
}

.food-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.food-name {
  font-size: 15px;
  font-weight: 600;
  color: #fff;
}

.food-desc {
  font-size: 11px;
  color: rgba(255,255,255,0.5);
  margin-top: 3px;
}

.food-ingredients {
  display: flex;
  gap: 12px;
  margin-top: 6px;
}

.ing-icon {
  width: 20px;
  height: 20px;
}

.ing-labels {
  display: flex;
  gap: 12px;
  margin-top: 2px;
}

.ing-label {
  width: 20px;
  text-align: center;
  font-size: 9px;
  color: rgba(255,255,255,0.5);
}

.food-action-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2px;
  width: 70px;
  background: linear-gradient(135deg, rgba(255,239,184,0.15), rgba(255,239,184,0.05));
  border: 1px solid rgba(255,239,184,0.2);
  border-radius: 10px;
  padding: 10px;
  cursor: pointer;
  flex-shrink: 0;
}

.action-text {
  font-size: 12px;
  color: #ffefb8;
  font-weight: 500;
}

/* 底部按钮 */
.bottom-btns {
  display: flex;
  justify-content: center;
  gap: 12px;
  padding: 10px 16px;
  background: #000;
}

.btn-small, .btn-large {
  border-radius: 16px;
  text-align: center;
  cursor: pointer;
  transition: opacity 0.2s;
}

.btn-small {
  padding: 8px 16px;
  font-size: 12px;
  color: rgba(255,255,255,0.6);
  background: rgba(255,255,255,0.06);
  border: 1px solid rgba(255,255,255,0.1);
  min-width: 80px;
}

.btn-large {
  padding: 8px 28px;
  font-size: 12px;
  color: #ffefb8;
  background: rgba(255,239,184,0.1);
  border: 1px solid rgba(255,239,184,0.25);
  min-width: 120px;
}

/* 上滑提示 */
.swipe-hint {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  padding-bottom: 8px;
  background: #000;
}

.swipe-hint .up-arrow {
  font-size: 10px;
  color: rgba(255,255,255,0.35);
  animation: bounce 1.5s infinite;
}

.swipe-hint .hint-text {
  font-size: 10px;
  color: rgba(255,255,255,0.35);
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-3px); }
}

/* 运动弹窗 */
.exercise-modal {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
}

.exercise-popup {
  width: 300px;
  background: #1a1a2e;
  border-radius: 16px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
}

.exercise-header {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.exercise-title {
  font-size: 14px;
  font-weight: 600;
  color: #fff;
}

.exercise-close {
  font-size: 24px;
  color: rgba(255,255,255,0.5);
  cursor: pointer;
  line-height: 1;
}

.exercise-time-display {
  font-size: 48px;
  font-weight: 300;
  color: #ffefb8;
  font-family: 'Courier New', monospace;
}

.exercise-ring {
  width: 140px;
  height: 140px;
}

.exercise-ring svg {
  transform: rotate(-90deg);
}

.ring-bg {
  fill: none;
  stroke: rgba(255,255,255,0.1);
  stroke-width: 6;
}

.ring-progress {
  fill: none;
  stroke: #ffefb8;
  stroke-width: 6;
  stroke-linecap: round;
  transition: stroke-dashoffset 0.3s;
}

.exercise-steps {
  width: 100%;
  background: rgba(255,255,255,0.05);
  border-radius: 10px;
  padding: 12px;
}

.step-title {
  font-size: 12px;
  font-weight: 600;
  color: rgba(255,255,255,0.7);
  margin-bottom: 6px;
}

.step-item {
  font-size: 11px;
  color: rgba(255,255,255,0.6);
  line-height: 1.6;
  margin-bottom: 4px;
}

.exercise-controls {
  display: flex;
  gap: 12px;
}

.ctrl-btn {
  padding: 8px 24px;
  border-radius: 20px;
  border: none;
  font-size: 13px;
  cursor: pointer;
  background: #ffefb8;
  color: #333;
  font-weight: 500;
}

.ctrl-btn.reset {
  background: rgba(255,255,255,0.1);
  color: rgba(255,255,255,0.7);
}

/* 食物教程弹窗 */
.food-modal {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.85);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
}

.food-popup {
  width: 320px;
  max-height: 85vh;
  background: #1a1a2e;
  border-radius: 16px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  overflow-y: auto;
}

.food-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.food-title {
  font-size: 14px;
  font-weight: 600;
  color: #fff;
}

.food-close {
  font-size: 24px;
  color: rgba(255,255,255,0.5);
  cursor: pointer;
  line-height: 1;
}

.food-main-img {
  width: 100%;
  height: 140px;
  border-radius: 12px;
  overflow: hidden;
}

.food-main-img img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.food-ingredients-row {
  display: flex;
  justify-content: space-around;
  background: rgba(255,255,255,0.05);
  border-radius: 10px;
  padding: 10px;
}

.ing-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
}

.ing-item img {
  width: 32px;
  height: 32px;
}

.ing-item span {
  font-size: 10px;
  color: rgba(255,255,255,0.7);
}

.food-steps {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.food-step {
  display: flex;
  gap: 10px;
  background: rgba(255,255,255,0.03);
  border-radius: 8px;
  padding: 8px;
}

.step-num {
  width: 22px;
  height: 22px;
  background: #ffefb8;
  color: #333;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 11px;
  font-weight: 600;
  flex-shrink: 0;
}

.step-content {
  flex: 1;
}

.step-name {
  font-size: 12px;
  font-weight: 600;
  color: #fff;
  margin-bottom: 2px;
}

.step-desc {
  font-size: 11px;
  color: rgba(255,255,255,0.6);
  line-height: 1.4;
}

.food-done-btn {
  padding: 10px;
  border-radius: 20px;
  border: none;
  background: #ffefb8;
  color: #333;
  font-size: 13px;
  font-weight: 600;
  cursor: pointer;
  margin-top: 4px;
}
</style>