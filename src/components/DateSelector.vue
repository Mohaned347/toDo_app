<template>
  <div class="date-selector">
    <button v-for="(day, index) in days" :key="index" class="date-btn" :class="{ active: day.active }">
      {{ day.label }}
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      days: [
        { label: 'Thu', active: false },
        { label: 'Fri', active: false },
        { label: 'Sat', active: false },
        { label: 'Sun', active: true },
        { label: 'Mon', active: false },
        { label: 'Tue', active: false },
        { label: 'Wed', active: false }
      ]
    };
  }
};
</script>

<style scoped>
.date-selector {
  display: flex;
  justify-content: space-between;
  padding: 16px;
  background: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  margin-top: 12px;
}

.date-btn {
  padding: 10px 14px;
  border-radius: 6px;
  background: #e0e0e0;
  border: none;
  cursor: pointer;
}

.date-btn.active {
  background: #ffc107;
  color: white;
}
</style>
