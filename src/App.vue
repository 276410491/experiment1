<template>
  <div class="container">
    <!-- 基本信息展示 -->
    <div class="profile-section">
      <h2>个人信息</h2>
      <div class="info-grid">
        <div v-for="(value, key) in studentInfo" :key="key" class="info-item">
          <span class="label">{{ keyLabels[key] }}：</span>
          <span class="value">{{ value }}</span>
        </div>
      </div>
    </div>

    <!-- 成绩信息展示 -->
    <div class="score-section">
      <h2>课程成绩</h2>
      <div class="stats">
        <span>总分：{{ totalScore }}</span>
        <span>平均分：{{ averageScore.toFixed(2) }}</span>
      </div>
      <div class="course-list">
        <div v-for="(course, index) in courses" :key="course.code" class="course-card">
          <div class="course-info">
            <h3>{{ course.name }}（{{ course.code }}）</h3>
            <p>类型：{{ course.type }} | 学分：{{ course.credit }}</p>
            <div class="score-control">
              <span>成绩：{{ course.score }}</span>
              <button 
                @click="modifyScore(index, 10)" 
                :disabled="isDisabled"
                class="btn add">+</button>
              <button 
                @click="modifyScore(index, -10)" 
                :disabled="isDisabled"
                class="btn subtract">-</button>
            </div>
            <p>绩点：{{ calculateGP(course).toFixed(2) }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref, computed } from 'vue'

// 学生基本信息（请替换为真实信息）
const studentInfo = reactive({
  name: '吴宗昌',
  id: '2220500803',
  gender: '男',
  birthdate: '2004-08-09',
  hometown: '山西运城'
})

// 字段显示标签
const keyLabels = {
  name: '姓名',
  id: '学号',
  gender: '性别',
  birthdate: '出生日期',
  hometown: '籍贯'
}

// 课程数据（请替换为真实成绩）
const courses = reactive([
  { 
    code: '27054028', 
    name: '软件开发过程实践', 
    type: '必修', 
    credit: 3,
    score: 87
  },
  {
    code: '25054022',
    name: 'JAVA WEB',
    type: '必修',
    credit: 4,
    score: 81
  },
  {
    code: '25052015',
    name: 'Python',
    type: '必修',
    credit: 2,
    score: 77
  }
])

// 交互逻辑
const clickCount = ref(0)
const isDisabled = ref(false)
const randomThreshold = ref(Math.floor(Math.random() * 3) + 2) // 生成2-4的随机数

const modifyScore = (index, delta) => {
  if (isDisabled.value) return
  
  const newScore = courses[index].score + delta
  courses[index].score = Math.min(Math.max(newScore, 0), 100)
  
  clickCount.value++
  if (clickCount.value >= randomThreshold.value) {
    courses.forEach(course => {
      course.score = Math.max(course.score - 10, 0)
    })
    isDisabled.value = true
  }
}

// 计算属性
const totalScore = computed(() => {
  return courses.reduce((sum, course) => sum + course.score, 0)
})

const averageScore = computed(() => {
  return courses.length ? totalScore.value / courses.length : 0
})

// 绩点计算方法
const calculateGP = (course) => {
  return (course.score / 10 - 5) * course.credit
}
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 2rem auto;
  padding: 20px;
  font-family: 'Segoe UI', sans-serif;
}

.profile-section, .score-section {
  background: #ffffff;
  border-radius: 12px;
  box-shadow: 0 2px 15px rgba(0, 0, 0, 0.1);
  padding: 2rem;
  margin-bottom: 2rem;
}

.info-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1.5rem;
  margin-top: 1rem;
}

.info-item {
  display: flex;
  justify-content: space-between;
  padding: 0.8rem;
  background: #f8f9fa;
  border-radius: 8px;
}

.course-list {
  display: grid;
  gap: 1.5rem;
  margin-top: 1.5rem;
}

.course-card {
  padding: 1.5rem;
  border: 1px solid #e0e0e0;
  border-radius: 10px;
  transition: transform 0.2s;
}

.course-card:hover {
  transform: translateY(-3px);
}

.score-control {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin: 1rem 0;
}

.btn {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: opacity 0.2s;
}

.btn.add {
  background: #4CAF50;
  color: white;
}

.btn.subtract {
  background: #f44336;
  color: white;
}

.btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}
.abc{
  /*ceshi*/
}
.stats {
  display: flex;
  gap: 2rem;
  margin: 1rem 0;
  font-weight: 500;
  color: #2c3e50;
}

h2, h3 {
  color: #34495e;
  margin-bottom: 1rem;
}

.label {
  font-weight: 600;
  color: #34495e;
}

.value {
  color: #7f8c8d;
}
</style>