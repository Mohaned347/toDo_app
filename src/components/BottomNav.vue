<template>
  <div class="bottom-nav">
    <button class="nav-btn">
      <img src="#" alt="Home" class="nav-icon">
    </button>
    <button class="add-btn">+</button>
    <button class="nav-btn">
      <img src="#" alt="Notifications" class="nav-icon">
    </button>
  </div>
</template>

<style scoped>
.bottom-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: space-around;
  padding: 16px;
  background: white;
  box-shadow: 0 -2px 4px rgba(0, 0, 0, 0.1);
}

.nav-btn {
  padding: 10px;
  background: none;
  border: none;
}

.nav-icon {
  width: 24px;
  height: 24px;
}

.add-btn {
  width: 40px;
  height: 40px;
  background: #ffc107;
  color: white;
  border-radius: 50%;
  border: none;
  font-size: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
