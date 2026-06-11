<script setup>
import { ref, computed } from 'vue'
import bgImage from '@/assets/cat.png'

// 建立貓咪必備用品的響應式陣列（第一週預計的 v-for 功能）
const supplies = ref([
  {
    id: 1,
    category: '生活起居',
    name: '雙層防帶砂貓砂盆',
    note: '幼貓骨骼仍在發育，記得選邊緣較低、方便牠爬進爬出的款式。',
    bought: false,
    priority: '🔥 遷入前必備'
  },
  {
    id: 2,
    category: '每日飲食',
    name: '天然無穀幼貓飼料',
    note: '幼貓成長期需要高能量與優質蛋白質，顆粒要選小顆粒才好咀嚼。',
    bought: false,
    priority: '🔥 遷入前必備'
  },
  {
    id: 3,
    category: '每日飲食',
    name: '幼貓發育成長主食罐',
    note: '補充水分的關鍵！質地通常比成貓罐更細緻（慕斯狀）方便幼貓舔食。',
    bought: false,
    priority: '🔥 遷入前必備'
  },
  {
    id: 4,
    category: '醫療出行',
    name: '安全鎖扣硬殼外出籠',
    note: '帶幼貓去醫院打預防針必備。建議選上方可開啟的款式，醫生較好抱貓。',
    bought: false,
    priority: '🔥 遷入前必備'
  },
  {
    id: 5,
    category: '生活起居',
    name: '天然環保豆腐貓砂',
    note: '幼貓容易因為好奇而誤食貓砂，建議使用天然原料製成的豆腐砂較安全。',
    bought: false,
    priority: '🔥 遷入前必備'
  },
  {
    id: 6,
    category: '生活起居',
    name: '劍麻貓抓板與抓柱',
    note: '從小培養在固定地方磨爪的習慣，能有效保護家中的沙發與家具。',
    bought: false,
    priority: '🌱 遷入後補齊'
  },
  {
    id: 7,
    category: '醫療出行',
    name: '幼貓專用安全指甲剪',
    note: '幼貓指甲長得極快，每週需定期修剪。從小減敏剪指甲，長大才不會抗拒。',
    bought: false,
    priority: '🌱 遷入後補齊'
  },
  {
    id: 8,
    category: '每日飲食',
    name: '寵物專用自動活水機',
    note: '流動的水能大幅提升幼貓喝水的意願，預防未來泌尿系統的疾病。',
    bought: false,
    priority: '🌱 遷入後補齊'
  }
])

// 頁籤狀態
const currentTab = ref('home')

// Modal 状态与当前视频
const isModalOpen = ref(false)
const currentVideoEmbedUrl = ref('')

const openVideo = (url) => {
  currentVideoEmbedUrl.value = url
  isModalOpen.value = true
}

const closeModal = () => {
  isModalOpen.value = false
  currentVideoEmbedUrl.value = ''
}

// 精選教學影片
const videos = ref([
  {
    id: 1,
    title: '幼貓互動與社會化指南',
    description: '學習如何與 0-6 個月幼貓正確互動',
    url: 'https://www.youtube.com/embed/gMO0NwMwEjs'
  },
  {
    id: 2,
    title: '新手餵食與拍嗝教學',
    description: '針對斷奶期幼貓的飲食與照護技巧',
    url: 'https://www.youtube.com/embed/r2xSejSpfZc'
  }
])

// 幼貓年齡健康提醒器
const selectedAge = ref('1')

const ageReminders = {
  '1': '🍼 照護重點：母乳或離乳慕斯轉型期。此時免疫力極低，需注意保暖，暫時不要洗澡！',
  '2': '💉 照護重點：開始適應乾固體飼料。記得帶去獸醫院進行全身檢查，並施打貓咪三合一疫苗第一劑！',
  '3': '🐛 照護重點：食量與活動力大增。需施打三合一疫苗第二劑，並開始進行每月的定期體內外驅蟲！',
  '4-6': '🐾 照護重點：進入換牙期與性成熟期。需與醫生預約安排結紮（絕育）手術，並準備換成成貓飲食！'
}

const currentAgeReminder = computed(() => ageReminders[selectedAge.value] || '')

// 自訂備忘錄
const todos = ref([
  { id: 1, text: '預約下週六打晶片', done: false },
  { id: 2, text: '購買幼貓新鮮食材', done: false }
])

const newTodoText = ref('')

const addTodo = () => {
  if (newTodoText.value.trim() === '') return
  const newId = todos.value.length > 0 ? Math.max(...todos.value.map(t => t.id)) + 1 : 1
  todos.value.push({ id: newId, text: newTodoText.value, done: false })
  newTodoText.value = ''
}

const removeTodo = (id) => {
  const index = todos.value.findIndex(t => t.id === id)
  if (index > -1) {
    todos.value.splice(index, 1)
  }
}

const handleTodoKeydown = (event) => {
  if (event.key === 'Enter') {
    addTodo()
  }
}
</script>

<template>
  <div class="app-container" :style="{ backgroundImage: `url(${bgImage})` }">
    <header class="site-header">
      <h1>🐾 新手貓奴的肉球筆記</h1>
      <p class="subtitle">應對 0 - 6 個月幼貓的手忙腳亂全攻略</p>
    </header>

    <!-- 導覽列 -->
  <nav class="nav-bar">
    <button 
      class="nav-btn" 
      :class="{ active: currentTab === 'home' }" 
      @click="currentTab = 'home'"
    >
      🏠 關於筆記
    </button>
    <button 
      class="nav-btn" 
      :class="{ active: currentTab === 'list' }" 
      @click="currentTab = 'list'"
    >
      🐾 必備用品清單
    </button>
    <button 
      class="nav-btn" 
      :class="{ active: currentTab === 'videos' }" 
      @click="currentTab = 'videos'"
    >
      📺 幼貓照顧影片
    </button>
    <button 
      class="nav-btn" 
      :class="{ active: currentTab === 'todo' }" 
      @click="currentTab = 'todo'"
    >
      📝 貓奴備忘錄
    </button>
  </nav>

  <main class="container">
    <!-- 主頁 -->
    <div v-if="currentTab === 'home'">
      <section class="intro-box">
        <h2>👋 新手貓奴村，歡迎你！</h2>
        <p>「原來養幼貓沒有想像中那麼可怕！跟著下面這個清單一項一項準備，你也能安心當個稱職的貓爸爸、貓媽媽。」</p>
      </section>

      <section class="home-features">
        <div class="feature-card">
          <h3>🐾 精選必備物資</h3>
          <p>備妥食衣住行 8 大核心用品，新手上路不慌張。</p>
        </div>
        <div class="feature-card">
          <h3>✔️ 進度動態更新</h3>
          <p>買好一項勾選一項，即時掌握你的準備進度。</p>
        </div>
        <div class="feature-card">
          <h3>📺 影音實務教學</h3>
          <p>精選 YouTube 示範影片，動態學習更輕鬆。</p>
        </div>
      </section>

      <section class="age-calculator">
        <h3>🎂 幼貓年齡健康提醒器</h3>
        <div class="age-selector">
          <label for="age-select">我的幼貓目前年齡：</label>
          <select v-model="selectedAge" id="age-select" class="age-dropdown">
            <option value="1">1個月</option>
            <option value="2">2個月</option>
            <option value="3">3個月</option>
            <option value="4-6">4-6個月</option>
          </select>
        </div>
        <div class="age-reminder">
          {{ currentAgeReminder }}
        </div>
      </section>

      <section class="home-footer">
        <p>💡 今日貓奴金句：用成倍的耐心與愛，陪伴小幼貓探索這個新世界。</p>
      </section>
    </div>

    <!-- 必備用品清單分頁 -->
    <div v-else-if="currentTab === 'list'">
      <section class="supplies-section">
        <h3>📦 第一步：帶貓回家前的必備用品清單</h3>
        
        <div class="card-grid">
          <div v-for="item in supplies" :key="item.id" class="supply-card" :class="{ 'is-bought': item.bought }">
            <div class="card-header">
              <input type="checkbox" v-model="item.bought" class="checkbox" />
              <span class="badge">{{ item.category }}</span>
              <span class="priority-badge">{{ item.priority }}</span>
            </div>
            <h4 class="card-title">{{ item.name }}</h4>
            <p class="card-note">{{ item.note }}</p>
          </div>
        </div>
      </section>
    </div>

    <!-- 精選教學影片分頁 -->
    <div v-else-if="currentTab === 'videos'">
      <section class="intro-box">
        <h2>🎥 精選教學影片</h2>
        <p>「觀看這些專業教學影片，快速掌握幼貓照顧的訣竅！」</p>
      </section>

      <section class="videos-section">
        <div class="videos-grid">
          <div v-for="video in videos" :key="video.id" class="video-card">
            <h4 class="video-title">{{ video.title }}</h4>
            <p class="video-description">{{ video.description }}</p>
            <button @click="openVideo(video.url)" class="video-link">觀看範例影片</button>
          </div>
        </div>
      </section>
    </div>

    <!-- 貓奴備忘錄分頁 -->
    <div v-else-if="currentTab === 'todo'">
      <section class="intro-box">
        <h2>📝 貓奴備忘錄</h2>
        <p>「記錄所有關於幼貓的重要事項，永遠不再遺忘任何一個細節！」</p>
      </section>

      <section class="todo-section">
        <div class="todo-input-container">
          <input 
            v-model="newTodoText" 
            @keydown="handleTodoKeydown"
            type="text" 
            placeholder="請輸入自訂備忘事項..." 
            class="todo-input"
          />
          <button @click="addTodo" class="todo-add-btn">➕ 新增</button>
        </div>

        <div class="todo-list">
          <div v-if="todos.length === 0" class="todo-empty">
            <p>目前沒有備忘事項，來新增一個吧！</p>
          </div>
          <div v-for="todo in todos" :key="todo.id" class="todo-item" :class="{ 'todo-done': todo.done }">
            <input type="checkbox" v-model="todo.done" class="todo-checkbox" />
            <span class="todo-text">{{ todo.text }}</span>
            <button @click="removeTodo(todo.id)" class="todo-delete-btn">❌ 刪除</button>
          </div>
        </div>
      </section>
    </div>
  </main>

  <div v-if="isModalOpen" class="modal-overlay" @click.self="closeModal">
    <div class="modal-content">
      <button class="modal-close" @click="closeModal">❌</button>
      <iframe :src="currentVideoEmbedUrl" width="100%" height="315" frameborder="0" allowfullscreen></iframe>
    </div>
  </div>
  </div>
</template>

<style scoped>
/* 整體溫馨橘色調樣式設計 */
.site-header {
  text-align: center;
  background-color: #fff3e0;
  padding: 40px 20px;
  border-radius: 12px;
  margin-bottom: 0;
  box-shadow: 0 4px 6px rgba(211, 84, 0, 0.05);
}

h1 {
  color: #e65100;
  margin: 0 0 10px 0;
  font-size: 2.2rem;
}

.subtitle {
  color: #f57c00;
  margin: 0;
  font-size: 1.1rem;
}

.container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 0 20px;
}

.intro-box {
  background-color: #fffde7;
  border: 2px dashed #ffb74d;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 40px;
  text-align: center;
  color: #5d4037;
}

.home-features {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 18px;
  margin-bottom: 24px;
}

.feature-card {
  background-color: #ffffff;
  border: 1px solid #ffe0b2;
  border-radius: 14px;
  padding: 20px;
  box-shadow: 0 4px 8px rgba(255, 167, 38, 0.12);
  transition: transform 0.25s ease, box-shadow 0.25s ease;
}

.feature-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 18px rgba(255, 167, 38, 0.16);
}

.feature-card h3 {
  margin: 0 0 10px 0;
  color: #e65100;
  font-size: 1.05rem;
}

.feature-card p {
  margin: 0;
  color: #6d4c41;
  line-height: 1.6;
}

.home-footer {
  background-color: #fff7ed;
  border: 1px solid #ffe0b2;
  border-radius: 12px;
  padding: 16px 20px;
  text-align: center;
  color: #8c7b6e;
  font-size: 0.95rem;
}

/* 幼貓年齡健康提醒器樣式 */
.age-calculator {
  background-color: #ffffff;
  border: 2px solid #ffb74d;
  border-radius: 12px;
  padding: 24px;
  margin-bottom: 24px;
  box-shadow: 0 4px 12px rgba(255, 167, 38, 0.1);
}

.age-calculator h3 {
  color: #e65100;
  margin: 0 0 16px 0;
  font-size: 1.2rem;
  text-align: center;
}

.age-selector {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  margin-bottom: 16px;
  flex-wrap: wrap;
}

.age-selector label {
  color: #5d4037;
  font-weight: 600;
  font-size: 1rem;
}

.age-dropdown {
  padding: 8px 12px;
  border: 2px solid #ffe0b2;
  border-radius: 6px;
  background-color: #fffde7;
  color: #5d4037;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.age-dropdown:hover {
  border-color: #ffb74d;
  box-shadow: 0 2px 6px rgba(255, 152, 0, 0.15);
}

.age-dropdown:focus {
  outline: none;
  border-color: #ff9800;
  box-shadow: 0 0 0 3px rgba(255, 152, 0, 0.1);
}

.age-reminder {
  background-color: #fff3e0;
  border-left: 4px solid #ff9800;
  padding: 16px;
  border-radius: 6px;
  color: #5d4037;
  font-size: 0.95rem;
  line-height: 1.6;
  text-align: center;
  font-weight: 500;
}

.supplies-section h3 {
  color: #e65100;
  border-bottom: 2px solid #ffe0b2;
  padding-bottom: 10px;
  margin-bottom: 20px;
}

/* 卡片排版 */
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 20px;
}

.supply-card {
  background-color: #ffffff;
  border: 1px solid #ffe0b2;
  border-radius: 10px;
  padding: 20px;
  position: relative;
  box-shadow: 0 2px 4px rgba(0,0,0,0.02);
  transition: transform 0.2s;
}

.supply-card:hover {
  transform: translateY(-5px);
  border-color: #ffb74d;
}

/* 分類標籤 */
.badge {
  background-color: #ffe0b2;
  color: #e65100;
  font-size: 0.8rem;
  padding: 4px 8px;
  border-radius: 4px;
  font-weight: bold;
}

.priority-badge {
  background-color: #fff3e0;
  color: #bf360c;
  font-size: 0.78rem;
  padding: 4px 8px;
  border-radius: 12px;
  border: 1px solid #ffe0b2;
  margin-left: auto;
}

.card-title {
  color: #3e2723;
  margin: 15px 0 10px 0;
  font-size: 1.2rem;
}

.card-note {
  color: #6d4c41;
  font-size: 0.95rem;
  line-height: 1.5;
  margin: 0;
}

/* 已購買狀態樣式 */
.card-header {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}

.checkbox {
  width: 18px;
  height: 18px;
  cursor: pointer;
  accent-color: #ff9800;
}

.is-bought {
  background-color: #f5f5f5;
  opacity: 0.7;
  border-color: #e0e0e0;
}

.is-bought .card-title,
.is-bought .card-note {
  text-decoration: line-through;
  color: #a1887f;
}

.is-bought .badge {
  background-color: #e8e8e8;
  color: #9e9e9e;
}

.app-container {
  min-height: 100vh;
  padding: 40px 0;
  position: relative;
  background-size: cover;
  background-position: center;
  background-attachment: fixed;
  background-repeat: no-repeat;
}

.app-container::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(255, 252, 247, 0.20);
  pointer-events: none;
  z-index: 0;
}

.app-container > * {
  position: relative;
  z-index: 1;
}

.nav-bar {
  background-color: rgba(255, 248, 240, 0.9);
  padding: 16px 22px;
  display: flex;
  gap: 12px;
  justify-content: center;
  border-bottom: 2px solid #ffe0b2;
  margin-bottom: 20px;
  border-radius: 16px;
  box-shadow: 0 10px 30px rgba(121, 85, 72, 0.08);
  backdrop-filter: blur(8px);
}

.nav-btn {
  background-color: #ffffff;
  border: 2px solid #ffb74d;
  color: #e65100;
  padding: 12px 24px;
  border-radius: 999px;
  cursor: pointer;
  font-size: 1rem;
  font-weight: 700;
  box-shadow: 0 6px 12px rgba(255, 183, 77, 0.18);
  transition: all 0.3s ease;
}

.nav-btn:hover {
  transform: translateY(-2px);
  background-color: #ffe0b2;
}

.nav-btn.active {
  background: linear-gradient(135deg, #ffb74d 0%, #ff9800 100%);
  color: #ffffff;
  box-shadow: 0 10px 20px rgba(255, 152, 0, 0.28);
}

/* 影片區塊樣式 */
.videos-section {
  margin-top: 30px;
}

.videos-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
}

.video-card {
  background-color: #ffffff;
  border: 2px solid #ffe0b2;
  border-radius: 10px;
  padding: 25px;
  box-shadow: 0 2px 8px rgba(211, 84, 0, 0.08);
  transition: all 0.3s ease;
  display: flex;
  flex-direction: column;
}

.video-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 8px 16px rgba(211, 84, 0, 0.15);
  border-color: #ffb74d;
}

.video-title {
  color: #3e2723;
  font-size: 1.3rem;
  margin: 0 0 10px 0;
  line-height: 1.4;
}

.video-description {
  color: #6d4c41;
  font-size: 0.95rem;
  line-height: 1.6;
  margin: 0 0 20px 0;
  flex-grow: 1;
}

.video-link {
  display: inline-block;
  background-color: #ff9800;
  color: #ffffff;
  padding: 12px 20px;
  border-radius: 6px;
  text-decoration: none;
  font-weight: bold;
  text-align: center;
  transition: all 0.3s ease;
  cursor: pointer;
  border: none;
}

.video-link:hover {
  background-color: #e65100;
  box-shadow: 0 4px 8px rgba(230, 81, 0, 0.3);
  transform: scale(1.05);
}

.modal-overlay {
  position: fixed;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.72);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  z-index: 999;
}

/* 貓奴備忘錄樣式 */
.todo-section {
  margin-top: 30px;
}

.todo-input-container {
  display: flex;
  gap: 10px;
  margin-bottom: 24px;
  flex-wrap: wrap;
}

.todo-input {
  flex: 1;
  min-width: 200px;
  padding: 12px 16px;
  border: 2px solid #ffe0b2;
  border-radius: 8px;
  font-size: 1rem;
  color: #5d4037;
  background-color: #fffde7;
  transition: all 0.3s ease;
}

.todo-input:focus {
  outline: none;
  border-color: #ffb74d;
  box-shadow: 0 0 0 3px rgba(255, 152, 0, 0.1);
}

.todo-input::placeholder {
  color: #a1887f;
}

.todo-add-btn {
  padding: 12px 24px;
  background-color: #ff9800;
  color: #ffffff;
  border: none;
  border-radius: 8px;
  font-weight: bold;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 8px rgba(255, 152, 0, 0.2);
}

.todo-add-btn:hover {
  background-color: #e65100;
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(230, 81, 0, 0.25);
}

.todo-add-btn:active {
  transform: translateY(0);
}

.todo-list {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.todo-empty {
  text-align: center;
  color: #a1887f;
  font-size: 1rem;
  padding: 40px 20px;
  background-color: #fff7ed;
  border: 1px dashed #ffe0b2;
  border-radius: 8px;
}

.todo-item {
  background-color: #ffffff;
  border: 1px solid #ffe0b2;
  border-radius: 8px;
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 12px;
  transition: all 0.3s ease;
}

.todo-item:hover {
  border-color: #ffb74d;
  box-shadow: 0 4px 8px rgba(255, 167, 38, 0.1);
}

.todo-checkbox {
  width: 20px;
  height: 20px;
  cursor: pointer;
  accent-color: #ff9800;
  flex-shrink: 0;
}

.todo-text {
  flex: 1;
  color: #5d4037;
  font-size: 1rem;
  line-height: 1.4;
}

.todo-done {
  background-color: #f5f5f5;
  opacity: 0.7;
  border-color: #e0e0e0;
}

.todo-done .todo-text {
  text-decoration: line-through;
  color: #a1887f;
}

.todo-delete-btn {
  padding: 8px 14px;
  background-color: #ffebee;
  color: #c62828;
  border: 1px solid #ff8a80;
  border-radius: 6px;
  font-size: 0.9rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
  flex-shrink: 0;
}

.todo-delete-btn:hover {
  background-color: #ffcdd2;
  border-color: #ef5350;
  box-shadow: 0 2px 6px rgba(244, 67, 54, 0.2);
}

.todo-delete-btn:active {
  transform: scale(0.95);
}

.modal-content {
  background-color: #ffffff;
  border-radius: 18px;
  max-width: 840px;
  width: 100%;
  padding: 20px;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.2);
  position: relative;
}

.modal-close {
  position: absolute;
  top: 18px;
  right: 18px;
  background: transparent;
  border: none;
  font-size: 1.2rem;
  cursor: pointer;
  color: #5d4037;
}

.modal-content iframe {
  border-radius: 14px;
  border: none;
}
</style>