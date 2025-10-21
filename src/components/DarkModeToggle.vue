<template>
  <button class="darkModeToggle" @click="toggleDarkMode">
    <img :src="isDark ? sunIcon : moonIcon" alt="Toggle Dark Mode" class="icon" />
  </button>
</template>

<script setup lang="ts">
import sunIcon from '@/assets/icons/sun.png'
import moonIcon from '@/assets/icons/moon.png'
import { onMounted, ref } from 'vue'

const isDark = ref(false)

const toggleDarkMode = () => {
  isDark.value = !isDark.value
  document.documentElement.classList.toggle('darkMode', isDark.value)
  localStorage.setItem('darkMode', isDark.value.toString())
}

onMounted(() => {
  const saved = localStorage.getItem('darkMode')
  if (saved === 'true') {
    isDark.value = true
    document.documentElement.classList.add('darkMode')
  }
})
</script>

<style scoped>
.darkModeToggle {
  position: fixed;
  bottom: 24px;
  right: 24px;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background-color: var(--accent);
  border: none;
  cursor: pointer;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
  display: flex;
  align-items: center;
  justify-content: center;
  transition:
    background-color 0.3s,
    transform 0.2s;
  z-index: 1000;
  padding: 0;
}

.darkModeToggle:hover {
  background-color: var(--accent-hover);
  transform: scale(1.05);
}

.icon {
  width: 24px;
  height: 24px;
}
</style>
