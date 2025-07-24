<script setup>
import { ref } from 'vue';
import { beverages } from './assets/beverages.js';
import { wzryHero } from './assets/wzryHero.js';

const firstName = ref('');
const lastName = ref('');
const generatedName = ref('');
const isCopied = ref(false);

const generateName = () => {
  firstName.value = beverages[Math.floor(Math.random() * beverages.length)];
  lastName.value = wzryHero[Math.floor(Math.random() * wzryHero.length)];
  generatedName.value = `${firstName.value}${lastName.value}`;
  isCopied.value = false; // 重置复制状态
};

// 初始生成一个名称
generateName();

const copyName = async () => {
  try {
    await navigator.clipboard.writeText(generatedName.value);
    isCopied.value = true;
    // 2秒后重置状态
    setTimeout(() => {
      isCopied.value = false;
    }, 2000);
  } catch (err) {
    console.error('复制失败:', err);
    // 可以在这里添加错误提示
  }
};
</script>

<template>
  <div class="container">
    <header class="header">
      <h1>旺仔小乔名称生成器</h1>
      <p class="subtitle">生成你的专属游戏ID</p>
    </header>
    
    <main class="main">
      <div class="name-display">
        <div class="name-box" :class="{ 'copied': isCopied }">
          {{ generatedName }}
          <div v-if="isCopied" class="copied-indicator">已复制!</div>
        </div>
      </div>
      
      <div class="controls">
        <button class="generate-btn" @click="generateName">
          <span>生成名称</span>
        </button>
        <button class="copy-btn" @click="copyName">
          <span>{{ isCopied ? '已复制' : '复制名称' }}</span>
        </button>
      </div>
      
      <div class="name-parts">
        <div class="part">
          <span class="part-label">饮品:</span>
          <span class="part-value">{{ firstName }}</span>
        </div>
        <div class="part">
          <span class="part-label">英雄:</span>
          <span class="part-value">{{ lastName }}</span>
        </div>
      </div>
    </main>
    
    <footer class="footer">
      <p>点击"生成名称"获取新ID</p>
    </footer>
  </div>
</template>

<style scoped>
.container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background: linear-gradient(135deg, #ffd1dc, #c9e9ff);
  font-family: 'Arial', sans-serif;
  padding: 20px;
  box-sizing: border-box;
  animation: gradientShift 10s ease infinite;
  background-size: 200% 200%;
}

@keyframes gradientShift {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

.header {
  text-align: center;
  margin-bottom: 30px;
  animation: fadeInDown 0.8s ease-out;
}

@keyframes fadeInDown {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.header h1 {
  font-size: 28px;
  color: #ff6b9d;
  margin: 20px 0 10px;
  text-shadow: 1px 1px 2px rgba(0,0,0,0.1);
  position: relative;
  display: inline-block;
}

.header h1::after {
  content: '';
  position: absolute;
  bottom: -5px;
  left: 50%;
  transform: translateX(-50%);
  width: 60px;
  height: 3px;
  background: linear-gradient(to right, #ff7eb3, #ff758c);
  border-radius: 3px;
}

.subtitle {
  font-size: 16px;
  color: #666;
  margin: 0;
  animation: fadeIn 1s ease-out 0.3s both;
}

.main {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 30px;
}

.name-display {
  width: 100%;
  display: flex;
  justify-content: center;
  perspective: 1000px;
}

.name-box {
  background: white;
  border-radius: 16px;
  padding: 30px 50px;
  font-size: 32px;
  font-weight: bold;
  color: #ff6b9d;
  box-shadow: 0 8px 20px rgba(0,0,0,0.1);
  text-align: center;
  min-width: 280px;
  border: 3px dashed #ffd1dc;
  position: relative;
  transition: all 0.3s ease;
  transform-style: preserve-3d;
}

.name-box.copied {
  animation: copyPulse 0.5s ease;
}

@keyframes copyPulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.05); }
  100% { transform: scale(1); }
}

.copied-indicator {
  position: absolute;
  top: -15px;
  right: -15px;
  background: #4caf50;
  color: white;
  padding: 5px 10px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: bold;
  box-shadow: 0 2px 8px rgba(0,0,0,0.2);
  animation: popIn 0.3s ease-out;
}

@keyframes popIn {
  0% { 
    opacity: 0;
    transform: scale(0.5);
  }
  70% { 
    opacity: 1;
    transform: scale(1.1);
  }
  100% { 
    transform: scale(1);
  }
}

.controls {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
  justify-content: center;
  animation: fadeInUp 0.8s ease-out 0.4s both;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.generate-btn, .copy-btn {
  padding: 14px 28px;
  font-size: 18px;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: bold;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  position: relative;
  overflow: hidden;
}

.generate-btn {
  background: linear-gradient(to right, #ff7eb3, #ff758c);
  color: white;
}

.generate-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(0,0,0,0.15);
}

.generate-btn:active {
  transform: translateY(0);
}

.copy-btn {
  background: white;
  color: #ff6b9d;
  border: 2px solid #ff6b9d;
}

.copy-btn:hover {
  background: #fff5f8;
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(0,0,0,0.15);
}

.copy-btn:active {
  transform: translateY(0);
}

.name-parts {
  width: 100%;
  background: rgba(255, 255, 255, 0.8);
  border-radius: 16px;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.05);
  animation: fadeIn 1s ease-out 0.6s both;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

.part {
  display: flex;
  justify-content: space-between;
  margin: 10px 0;
  font-size: 18px;
  transition: all 0.3s ease;
}

.part:hover {
  transform: translateX(5px);
}

.part-label {
  font-weight: bold;
  color: #ff6b9d;
}

.part-value {
  color: #333;
  font-weight: 500;
}

.footer {
  text-align: center;
  margin-top: 30px;
  color: #666;
  font-size: 14px;
  animation: fadeIn 1s ease-out 0.8s both;
}

@media (max-width: 480px) {
  .container {
    padding: 15px;
  }
  
  .header h1 {
    font-size: 24px;
  }
  
  .name-box {
    padding: 20px 30px;
    font-size: 28px;
    min-width: 240px;
  }
  
  .generate-btn, .copy-btn {
    padding: 12px 24px;
    font-size: 16px;
  }
  
  .part {
    font-size: 16px;
  }
}
</style>