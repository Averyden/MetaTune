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
import { onMounted, ref } from 'vue'

export default {
  setup() {
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
        document.documentElement.classList.add('dark')
      }
    })

    return {
      isDark,
      toggleDarkMode,
    }
  },
}
</script>
