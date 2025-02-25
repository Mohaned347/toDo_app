<template>
  <div class="task-list">
    <h2 class="task-title">Today's tasks</h2>
    <div v-for="task in tasks" :key="task.text" class="task-item">
      <div>
        <p class="task-time">{{ task.time }}</p>
        <p class="task-text">{{ task.text }}</p>
      </div>
      <input type="checkbox" v-model="task.completed" class="task-checkbox">
    </div>
  </div>
</template>

<script>
export default {
  props: ['tasks']
};
</script>

<style scoped>
.task-list {
  margin-top: 16px;
}

.task-title {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 12px;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 8px;
}

.task-time {
  color: #6c757d;
}

.task-text {
  font-size: 16px;
  font-weight: 600;
}

.task-checkbox {
  width: 20px;
  height: 20px;
}
</style>
