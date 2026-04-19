<script setup>
// 從 Vue 盒子工廠拿幾個小盒子來用
import { ref } from 'vue'

// 這是一個小盒子，用來存放用戶打的名字
const nameBox = ref('')

// 這是一個小盒子，用來存放找到的照片網址
const photoBox = ref('')

// 這是一個小盒子，用來存放 GitHub 個人頁面連結
const linkBox = ref('')

// 這是一個小盒子，用來記錄現在是不是正在 loading
const isLoadingBox = ref(false)

// 這是一個小盒子，用來記錄有沒有找錯人
const errorBox = ref('')

// 這是一個小遊戲，當按鈕被按下時會執行
function searchGame() {
  // 如果沒有輸入名字，就不要繼續
  if (!nameBox.value) return

  // 把 loading 盒子打開
  isLoadingBox.value = true
  // 把照片盒子先清空
  photoBox.value = ''
  // 把連結盒子先清空
  linkBox.value = ''
  // 把錯誤盒子先清空
  errorBox.value = ''

  // 去 GitHub 找這個人
  fetch('https://api.github.com/users/' + nameBox.value)
    .then(response => {
      if (!response.ok) {
        throw new Error('找不到這個人QQ')
      }
      return response.json()
    })
    .then(data => {
      // 把找到的照片網址放進盒子裡
      photoBox.value = data.avatar_url
      // 把找到的 GitHub 連結放進盒子裡
      linkBox.value = data.html_url
      // 把 loading 盒子關掉
      isLoadingBox.value = false
    })
    .catch(error => {
      // 如果找不到就把錯誤訊息放進錯誤盒子
      errorBox.value = error.message
      // 把 loading 關掉
      isLoadingBox.value = false
    })
}
</script>

<template>
  <div class="container">
    <h1>GitHub 偵察機 🔍</h1>
    
    <div class="search-box">
      <!-- 這是輸入框，用來打名字 -->
      <input 
        v-model="nameBox" 
        type="text" 
        placeholder="打一個 GitHub 名字試試看"
        @keyup.enter="searchGame"
      />
      
      <!-- 這是按鈕，按下去會開始找照片 -->
      <button @click="searchGame">搜尋</button>
    </div>

    <!-- 這裡會顯示 loading -->
    <p v-if="isLoadingBox" class="loading">正在找人中...</p>

    <!-- 這裡會顯示錯誤訊息 -->
    <p v-if="errorBox" class="error">{{ errorBox }}</p>

    <!-- 這裡會顯示找到的照片 -->
    <img v-if="photoBox" :src="photoBox" alt="avatar" class="avatar" />
    
    <!-- 這裡會顯示 GitHub 連結 -->
    <a v-if="linkBox" :href="linkBox" target="_blank" class="profile-link">
      查看 GitHub 個人頁面 👆
    </a>
  </div>
</template>

<style>
/* 夢幻紫風格 */
body {
  margin: 0;
  min-height: 100vh;
  background: linear-gradient(135deg, #a855f7 0%, #6366f1 50%, #8b5cf6 100%);
}

.container {
  text-align: center;
  padding: 50px;
  font-family: 'Comic Sans MS', 'Microsoft JhengHei', cursive;
  color: white;
}

h1 {
  font-size: 48px;
  text-shadow: 0 0 20px rgba(255,255,255,0.5);
  margin-bottom: 40px;
}

.search-box {
  margin: 20px 0;
}

input {
  padding: 15px 20px;
  font-size: 18px;
  width: 250px;
  border: none;
  border-radius: 30px;
  outline: none;
  font-family: inherit;
  box-shadow: 0 4px 15px rgba(0,0,0,0.2);
}

button {
  padding: 15px 35px;
  font-size: 18px;
  margin-left: 15px;
  cursor: pointer;
  border: none;
  border-radius: 30px;
  background: linear-gradient(90deg, #ec4899, #a855f7);
  color: white;
  font-weight: bold;
  font-family: inherit;
  transition: all 0.3s;
  box-shadow: 0 0 20px rgba(236, 72, 153, 0.5);
}

button:hover {
  transform: scale(1.05);
  box-shadow: 0 0 30px rgba(236, 72, 153, 0.8);
}

.avatar {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  margin-top: 30px;
  border: 6px solid white;
  box-shadow: 0 0 30px rgba(255,255,255,0.5);
  animation: appear 0.5s ease-out;
}

@keyframes appear {
  from {
    opacity: 0;
    transform: scale(0.5);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

.loading {
  color: white;
  font-size: 20px;
  margin-top: 20px;
  text-shadow: 0 0 10px rgba(255,255,255,0.5);
}

.error {
  color: #fecaca;
  font-size: 20px;
  margin-top: 20px;
  background: rgba(0,0,0,0.2);
  padding: 10px 20px;
  border-radius: 10px;
  display: inline-block;
}

.profile-link {
  display: inline-block;
  margin-top: 20px;
  color: white;
  text-decoration: none;
  background: rgba(255,255,255,0.2);
  padding: 12px 25px;
  border-radius: 25px;
  transition: all 0.3s;
}

.profile-link:hover {
  background: rgba(255,255,255,0.3);
  transform: scale(1.05);
}
</style>