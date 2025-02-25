<template>
  <div class="container">
    <header class="app-header">
      <div class="profile-section">
        <img 
          :src="photo" 
          alt="Profile" 
          class="profile-pic"
        >
        <div class="greeting-section">
          <p class="greeting-text">{{ dynamicGreeting }}</p>
          <h1 class="username">Mohaned</h1>
        </div>
      </div>
      <div class="header-controls">
        <button class="notification-btn">🔍</button>
        <button class="notification-btn">🔔</button>
      </div>
    </header>

    <div class="main-content">
      <div class="day-picker">
        <button 
          v-for="(day, index) in days" 
          :key="index"
          :class="['day-btn', { active: selectedDay === index }]"
          @click="selectDay(index)"
        >
          {{ day }}
        </button>
      </div>

      <div class="search-bar">
        <input 
          type="text" 
          v-model="searchQuery"
          placeholder="Search tasks..."
          class="search-input"
        >
      </div>

      <div class="tasks-section">
        <h2 class="section-title">{{ selectedDayName }} Tasks</h2>
        
        <div class="task-list">
          <div 
            class="task-item" 
            v-for="(task, index) in filteredTasks" 
            :key="index"
          >
            <div class="task-content">
              <input 
                type="checkbox" 
                v-model="task.completed"
                class="task-checkbox"
              >
              <span 
                class="task-time"
                :class="{ completed: task.completed }"
              >{{ task.time }}</span>
              <span 
                class="task-name"
                :class="{ completed: task.completed }"
              >{{ task.name }}</span>
            </div>
            <div class="task-actions">
              <button @click="editTask(index)" class="edit-btn">✏️</button>
              <button @click="deleteTask(index)" class="delete-btn">🗑️</button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <nav class="bottom-nav">
      <div class="nav-items">
        <button class="nav-btn" :class="{ active: activeNav === 0 }" @click="activeNav = 0">
          <font-awesome-icon :icon="['fas', 'home']" />
        </button>
        <button class="nav-btn" :class="{ active: activeNav === 1 }" @click="activeNav = 1">
          <font-awesome-icon :icon="['fas', 'calendar-alt']" />
        </button>
      </div>
      <button class="add-btn" @click="showModal = true">
        <font-awesome-icon :icon="['fas', 'plus']" />
      </button>
      <div class="nav-items">
        <button class="nav-btn" :class="{ active: activeNav === 2 }" @click="activeNav = 2">
          <font-awesome-icon :icon="['fas', 'bell']" />
        </button>
        <button class="nav-btn" :class="{ active: activeNav === 3 }" @click="activeNav = 3">
          <font-awesome-icon :icon="['fas', 'th-large']" />
        </button>
      </div>
    </nav>

    <div v-if="showModal" class="modal">
      <div class="modal-content">
        <h3>{{ editingIndex === null ? 'Add Task' : 'Edit Task' }}</h3>
        <div class="time-picker">
          <input 
            type="time" 
            v-model="newTask.time"
            class="time-input"
          >
        </div>
        <input 
          v-model="newTask.name" 
          type="text" 
          placeholder="Task name"
          class="task-name-input"
        >
        <div class="modal-actions">
          <button @click="saveTask" class="save-btn">💾 Save</button>
          <button @click="cancelEdit" class="cancel-btn">❌ Cancel</button>
        </div>
      </div>
    </div>
  </div>
</template>


<script setup>
import { ref, reactive, computed } from 'vue'
import photo from './assets/image.png'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { library } from '@fortawesome/fontawesome-svg-core'
import { faHome, faCalendarAlt, faPlus, faBell, faThLarge } from '@fortawesome/free-solid-svg-icons'

library.add(faHome, faCalendarAlt, faPlus, faBell, faThLarge)

const days = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
const selectedDay = ref(3)
const searchQuery = ref('')
const activeNav = ref(0)

const tasks = ref([
  { time: '08:00', name: 'Go to church', day: 3, completed: false },
  { time: '12:00', name: 'Cook for family', day: 3, completed: false },
  { time: '14:02', name: 'Workout session', day: 3, completed: false },
  { time: '17:00', name: 'Visit friend', day: 3, completed: false },
  { time: '18:00', name: 'Hair appointment', day: 3, completed: false },
  { time: '21:00', name: 'Call brother', day: 3, completed: false }
])

const showModal = ref(false)
const editingIndex = ref(null)
const newTask = reactive({
  time: '',
  name: '',
  day: selectedDay.value,
  completed: false
})

const dynamicGreeting = computed(() => {
  const hour = new Date().getHours()
  if (hour < 12) return 'Good morning'
  if (hour < 18) return 'Good afternoon'
  return 'Good evening'
})

const selectedDayName = computed(() => days[selectedDay.value])

const filteredTasks = computed(() => {
  return tasks.value.filter(task => 
    task.day === selectedDay.value &&
    task.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});

const selectDay = (index) => {
  selectedDay.value = index
  newTask.day = index
}

const saveTask = () => {
  if (!newTask.time || !newTask.name) return
  
  const formattedTask = {
    ...newTask,
    time: formatTime(newTask.time)
  }

  if (editingIndex.value === null) {
    tasks.value.push(formattedTask)
  } else {
    tasks.value[editingIndex.value] = formattedTask
  }
  
  cancelEdit()
}

const formatTime = (time) => {
  const [hours, minutes] = time.split(':')
  return `${hours.padStart(2, '0')}:${minutes}`
}

const editTask = (index) => {
  const task = tasks.value[index]
  newTask.time = task.time
  newTask.name = task.name
  editingIndex.value = index
  showModal.value = true
}

const deleteTask = (index) => {
  tasks.value.splice(index, 1)
}

const cancelEdit = () => {
  showModal.value = false
  editingIndex.value = null
  newTask.time = ''
  newTask.name = ''
}
</script>

<style>
:root {
  --bg-color: #0f0f0f;
  --surface-color: #1e1e1e;
  --primary-color: #bb86fc;
  --secondary-color: #03dac6;
  --text-primary: #ffffff;
  --text-secondary: #a0a0a0;
}

body {
  background-color: var(--bg-color);
  color: var(--text-primary);
  font-family: 'Inter', sans-serif;
  margin: 0;
  padding: 0;
}

.container {
  max-width: 100%;
  padding: 15px;
  padding-bottom: 80px;
}

.app-header {
  background: var(--surface-color);
  padding: 1rem;
  border-radius: 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.profile-section {
  display: flex;
  align-items: center;
  gap: 15px;
}

.profile-pic {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: 2px solid var(--primary-color);
  object-fit: cover;
}

.greeting-section {
  display: flex;
  flex-direction: column;
}

.greeting-text {
  font-size: 14px;
  opacity: 0.8;
  margin: 0;
}

.username {
  font-size: 18px;
  font-weight: 600;
  margin: 0;
}

.header-controls {
  display: flex;
  gap: 10px;
}

.notification-btn {
  background: rgba(255,255,255,0.05);
  border: none;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-primary);
  cursor: pointer;
  transition: all 0.3s ease;
}

.notification-btn:hover {
  background: rgba(255,255,255,0.1);
}
.day-picker {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 0.5rem;
  margin: 1rem 0;
}

.day-btn {
  background: var(--surface-color);
  border: none;
  padding: 0.8rem;
  border-radius: 12px;
  color: var(--text-primary);
  cursor: pointer;
  transition: all 0.2s ease;
  font-size: 14px;
}

.day-btn.active {
  background: var(--primary-color);
  color: #000;
  font-weight: bold;
}

.search-bar {
  margin: 1rem 0;
}

.search-input {
  width: 100%;
  padding: 1rem;
  background: var(--surface-color);
  border: none;
  border-radius: 25px;
  color: var(--text-primary);
  font-size: 1rem;
}

.tasks-section {
  background: var(--surface-color);
  border-radius: 20px;
  padding: 1.5rem;
  margin: 1rem 0;
}

.task-item {
  background: rgba(255,255,255,0.05);
  border-radius: 15px;
  margin: 0.8rem 0;
  padding: 1.2rem;
  transition: transform 0.2s ease;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task-item:hover {
  transform: translateX(10px);
  background: rgba(255,255,255,0.08);
}

.task-content {
  display: flex;
  align-items: center;
  gap: 1rem;
  flex-grow: 1;
}

.task-checkbox {
  width: 18px;
  height: 18px;
  accent-color: var(--primary-color);
}

.completed {
  text-decoration: line-through;
  opacity: 0.7;
}

.task-time {
  color: var(--text-secondary);
  font-size: 14px;
}

.task-name {
  color: var(--text-primary);
  font-weight: 500;
  font-size: 16px;
}

.task-actions {
  display: flex;
  gap: 8px;
}

.edit-btn, .delete-btn {
  border: none;
  background: none;
  font-size: 18px;
  cursor: pointer;
  padding: 4px;
  transition: all 0.3s ease;
}

.edit-btn:hover {
  color: var(--primary-color);
  transform: scale(1.1);
}

.delete-btn:hover {
  color: #dc3545;
  transform: scale(1.1);
}
.bottom-nav {
  background: #ffffff;
  box-shadow: 0 -4px 10px rgba(0, 0, 0, 0.1);
  position: fixed;
  bottom: 25px;
  left: 50%;
  transform: translateX(-50%);
  width: 90%;
  max-width: 400px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 20px;
  border-radius: 35px;
}


.nav-items {
  display: flex;
  gap: 30px;
}

.nav-btn {
  color: #bbb;
  font-size: 24px;
  border: none;
  background: none;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}

.nav-btn.active {
  color: #333;
  transform: scale(1.2);
}


.add-btn {
  background: linear-gradient(45deg, #f39c12, #e67e22);
  color: #fff;
  width: 65px;
  height: 65px;
  border-radius: 50%;
  border: none;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: -30px;
  font-size: 30px;
  cursor: pointer;
  box-shadow: 0px 5px 10px rgba(0, 0, 0, 0.2);
  transition: all 0.3s ease;
}

.add-btn:hover {
  transform: translateX(-50%) scale(1.1);
}
.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0,0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: var(--surface-color);
  color: var(--text-primary);
  padding: 25px;
  border-radius: 20px;
  width: 90%;
  max-width: 400px;
  box-shadow: 0 8px 32px rgba(0,0,0,0.1);
}

.task-name-input,
.time-input {
  background: rgba(255,255,255,0.05);
  color: var(--text-primary);
  border: 1px solid rgba(255,255,255,0.1);
  margin: 0.5rem 0;
  width: 100%;
  padding: 12px;
  border-radius: 8px;
  font-size: 16px;
}

.modal-actions {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

.save-btn, .cancel-btn {
  flex: 1;
  padding: 12px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  transition: all 0.3s ease;
}

.save-btn {
  background: var(--primary-color);
  color: #000;
}

.save-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(187,134,252,0.2);
}

.cancel-btn {
  background: rgba(255,255,255,0.05);
  color: var(--text-primary);
}

.cancel-btn:hover {
  background: rgba(255,255,255,0.1);
}

button:active {
  transform: scale(0.95);
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.modal-content {
  animation: fadeIn 0.3s ease;
}


@media (max-width: 480px) {
  .day-btn {
    padding: 0.6rem;
    font-size: 12px;
  }

  .profile-pic {
    width: 35px;
    height: 35px;
  }

  .username {
    font-size: 16px;
  }

  .nav-items {
    gap: 15px;
  }

  .nav-btn {
    font-size: 20px;
  }

  .add-btn {
    width: 50px;
    height: 50px;
    font-size: 28px;
    bottom: 20px;
  }
}
</style>