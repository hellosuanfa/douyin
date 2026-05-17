<template>
  <div class="device-wrapper">
    <div class="phone-frame">
      <div class="phone-screen">
        <div class="page">
          <!-- 顶部导航 - 抖音框架 -->
          <div class="top-nav">
            <span class="tab">团购</span>
            <span class="tab active">上海</span>
            <span class="tab">关注</span>
            <span class="tab">商城</span>
            <span class="tab">推荐</span>
          </div>

          <!-- 中间内容区 -->
          <div class="content">
            <div class="new-modules">
              <!-- 早晨风光 -->
              <div class="new-section">
                <div class="section-title">早晨风光</div>
                <div class="morning-scene">
                  <div class="scene-placeholder">风景图片</div>
                </div>
              </div>

              <!-- 今日运势 -->
              <div class="new-section">
                <div class="section-title">今日运势</div>
                <div class="info-card">
                  <div class="card-icon">🔮</div>
                  <div class="card-info">
                    <div class="card-name">今日运势</div>
                    <div class="card-desc">点击查看今日运势详情</div>
                  </div>
                  <div class="card-arrow">›</div>
                </div>
              </div>

              <!-- 早餐养生 -->
              <div class="new-section">
                <div class="section-title">早餐养生</div>
                <div class="info-card">
                  <div class="card-icon">🥣</div>
                  <div class="card-info">
                    <div class="card-name">养胃早餐</div>
                    <div class="card-desc">点击查看早餐推荐</div>
                  </div>
                  <div class="card-arrow">›</div>
                </div>
              </div>

              <!-- 今日穿搭 -->
              <div class="new-section">
                <div class="section-title">今日穿搭</div>
                <div class="info-card">
                  <div class="card-icon">👔</div>
                  <div class="card-info">
                    <div class="card-name">今日穿搭</div>
                    <div class="card-desc">根据天气推荐穿搭</div>
                  </div>
                  <div class="card-arrow">›</div>
                </div>
              </div>
            </div>
          </div>

          <!-- 底部导航 - 抖音框架 -->
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
</script>

<style scoped>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

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

.page {
  height: 100%;
  display: flex;
  flex-direction: column;
  font-family: -apple-system, BlinkMacSystemFont, "PingFang SC", sans-serif;
}

/* 顶部导航 */
.top-nav {
  display: flex;
  justify-content: center;
  gap: 18px;
  padding: 10px 16px;
}

.tab {
  font-size: 14px;
  color: rgba(255,255,255,0.5);
}

.tab.active {
  color: #fff;
  font-weight: 600;
}

/* 内容区 */
.content {
  flex: 1;
  overflow-y: auto;
  padding: 12px;
}

.new-modules {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.new-section {
  background: rgba(255,255,255,0.03);
  border-radius: 10px;
  padding: 10px;
}

.section-title {
  font-size: 13px;
  font-weight: 600;
  color: rgba(255,255,255,0.9);
  margin-bottom: 8px;
}

.morning-scene {
  height: 80px;
  background: rgba(255,255,255,0.05);
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.scene-placeholder {
  color: rgba(255,255,255,0.3);
  font-size: 12px;
}

.info-card {
  display: flex;
  align-items: center;
  gap: 10px;
  background: rgba(255,255,255,0.04);
  border-radius: 10px;
  padding: 12px;
}

.card-icon {
  font-size: 24px;
}

.card-info {
  flex: 1;
}

.card-name {
  font-size: 13px;
  font-weight: 500;
  color: #fff;
}

.card-desc {
  font-size: 11px;
  color: rgba(255,255,255,0.4);
  margin-top: 2px;
}

.card-arrow {
  font-size: 18px;
  color: rgba(255,255,255,0.3);
}

/* 底部导航 */
.bottom-nav {
  display: flex;
  justify-content: space-around;
  align-items: center;
  padding: 10px 16px 20px;
  background: #2c2c2e;
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
</style>