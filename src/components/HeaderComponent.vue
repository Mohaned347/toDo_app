<template>
  <div class="header-container">
    <img src="#" alt="Profile" class="profile-img">
    <div>
      <p class="greeting-text">Good evening,</p>
      <h2 class="username">Ivy</h2>
    </div>
  </div>
</template>

<style scoped>
.header-container {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 16px;
  background: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
}

.profile-img {
  width: 48px;
  height: 48px;
  border-radius: 50%;
}

.greeting-text {
  color: #6c757d;
}

.username {
  font-size: 18px;
  font-weight: bold;
}
</style>
