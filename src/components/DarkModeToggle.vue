<template>
  <button class="darkModeToggle" @click="toggleDarkMode">
    <img
      :src="isDark ? 'src/assets/icons/sun.png' : 'src/assets/icons/moon.png'"
      alt="Toggle Dark Mode"
      class="icon"
    />
  </button>
</template>

<script lang="ts">
import { ref } from 'vue'

const isDark = ref(false)

const toggleDarkMode = () => {
  isDark.value = !isDark.value
  document.documentElement.classList.toggle('darkMode', isDark.value)
  localStorage.setItem('darkMode', isDark.value.toString())
}
</script>
