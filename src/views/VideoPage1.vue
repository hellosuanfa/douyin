<template>
  <div class="page-container">
    <div class="video-wrapper">
      <div class="video-area">
        <video ref="videoRef" class="video-player" :src="videoSrc" @ended="onVideoEnd" @play="onVideoPlay" @pause="onVideoPause" playsinline></video>
        <div class="video-overlay" v-if="!isPlaying" @click="toggleVideo">
          <div class="play-btn">▶</div>
        </div>
        <div class="video-controls" v-if="videoDuration > 0">
          <div class="progress-bar" @click="seekVideo">
            <div class="progress-fill" :style="{ width: (videoCurrentTime / videoDuration * 100) + '%' }"></div>
          </div>
          <div class="time-info">{{ formatTime(videoCurrentTime) }} / {{ formatTime(videoDuration) }}</div>
        </div>
      </div>
    </div>
    <div class="swipe-hint" @click="$emit('swipe')">
      <span class="down-arrow">∨</span>
      <span class="hint-text">下滑切换</span>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue'

const videoSrc = '/5.17（1）.mp4'
const isPlaying = ref(false)
const videoRef = ref(null)
const videoDuration = ref(0)
const videoCurrentTime = ref(0)
let videoTimer = null

onMounted(() => {
  nextTick(() => {
    if (videoRef.value) {
      videoRef.value.play()
      isPlaying.value = true
      videoTimer = setInterval(() => {
        if (videoRef.value) {
          videoCurrentTime.value = videoRef.value.currentTime
        }
      }, 1000)
    }
  })
})

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

const onVideoEnd = () => {
  isPlaying.value = false
  clearInterval(videoTimer)
  videoCurrentTime.value = 0
}

const onVideoPlay = () => {
  videoDuration.value = videoRef.value.duration
}

const onVideoPause = () => {
  clearInterval(videoTimer)
}

const seekVideo = (e) => {
  const rect = e.currentTarget.getBoundingClientRect()
  const ratio = (e.clientX - rect.left) / rect.width
  if (videoRef.value && videoDuration.value > 0) {
    videoRef.value.currentTime = ratio * videoDuration.value
  }
}

const formatTime = (seconds) => {
  const m = Math.floor(seconds / 60)
  const s = Math.floor(seconds % 60)
  return `${m.toString().padStart(2, '0')}:${s.toString().padStart(2, '0')}`
}
</script>

<style scoped>
.page-container {
  height: 100%;
  display: flex;
  flex-direction: column;
  background: #000;
}

.video-wrapper {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.video-area {
  position: relative;
  width: 100%;
  height: 100%;
}

.video-player {
  width: 100%;
  height: 100%;
  object-fit: contain;
  background: #000;
}

.video-overlay {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0,0,0,0.4);
  cursor: pointer;
}

.play-btn {
  width: 60px;
  height: 60px;
  background: rgba(255,213,79,0.9);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  color: #333;
}

.video-controls {
  position: absolute;
  bottom: 80px;
  left: 16px;
  right: 16px;
}

.progress-bar {
  height: 4px;
  background: rgba(255,255,255,0.3);
  border-radius: 2px;
  overflow: hidden;
  cursor: pointer;
}

.progress-fill {
  height: 100%;
  background: #ffd54f;
}

.time-info {
  font-size: 12px;
  color: rgba(255,255,255,0.7);
  text-align: center;
  margin-top: 8px;
}

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
</style>