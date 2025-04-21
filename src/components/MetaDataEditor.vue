<template>
  <div class="modal" v-if="visible">
    <div class="modalContent" :class="{ closing: isClosing }" @animationend="handleAnimationEnd">
      <h2>Editing Metadata</h2>
      <label>Title:</label>
      <input v-model="editableMetaData.title" type="text" />

      <label>Artist:</label>
      <input v-model="editableMetaData.artist" type="text" />

      <label>Album:</label>
      <input v-model="editableMetaData.album" type="text" />

      <label>Album Cover:</label>
      <input type="file" accept="image/*" @change="handleImageUpload" />

      <div v-if="editableMetaData.coverUrl">
        <img :src="editableMetaData.coverUrl" class="coverPreview" />
      </div>

      <div class="buttonGroup">
        <button class="btnSave" @click="saveChanges">Save</button>
        <button class="btnClose" @click="closeModal">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue'

export default defineComponent({
  name: 'MetaDataEditor',
  props: ['metadata', 'visible'],
  setup(props, { emit }) {
    const editableMetaData = ref({ ...props.metadata })
    const isClosing = ref(false)

    watch(props.metadata, (newVal) => {
      editableMetaData.value = { ...newVal }
    })

    const handleImageUpload = (event: Event) => {
      const file = (event.target as HTMLInputElement).files?.[0]
      if (file) {
        const reader = new FileReader()
        reader.onload = () => {
          editableMetaData.value.coverUrl = reader.result as string
        }
        reader.readAsDataURL(file)
      }
    }

    const saveChanges = () => {
      closeModal()
      setTimeout(() => {
        emit('save', editableMetaData.value)
      }, 300)
    }

    const closeModal = () => {
      isClosing.value = true
    }

    const handleAnimationEnd = () => {
      if (isClosing.value) {
        emit('close')
        isClosing.value = false
      }
    }

    return {
      editableMetaData,
      saveChanges,
      closeModal,
      isClosing,
      handleAnimationEnd,
      handleImageUpload,
    }
  },
})
</script>

<style scoped>
/* TODO: Stylize this like the notesu popup :D */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: transparent;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: background-color 0.45s ease-in-out;
}

.modal.visible {
  background-color: rgba(0, 0, 0, 0.3);
}

@keyframes slideUp {
  0% {
    transform: translateY(0);
  }
  95% {
    opacity: 0;
  }
  100% {
    transform: translateY(-100%);
    visibility: hidden;
    opacity: 0;
  }
}

@keyframes slideDown {
  0% {
    transform: translateY(-100%);
    opacity: 0;
    visibility: visible;
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}

.modalContent {
  margin: auto;
  position: relative;
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
  align-items: stretch;
  animation: slideDown 0.3s ease forwards;
  padding: 24px;
  width: 90%;
  max-width: 420px;
}

.modalContent.closing {
  animation: slideUp 0.3s ease forwards;
}

.modalContent h2 {
  font-family: 'Roboto', sans-serif;
  font-weight: 700;
  font-size: 24px;
  text-align: center;
  padding-bottom: 12px;
  color: #222;
}

label {
  font-family: 'Roboto', sans-serif;
  font-size: 14px;
  font-weight: 500;
  color: #444;
  margin-top: 14px;
  margin-bottom: 4px;
  text-align: left;
}

input[type='text'],
input[type='file'] {
  font-family: 'Roboto', sans-serif;
  background-color: #f0f0f0;
  border: none;
  border-radius: 12px;
  padding: 12px;
  font-size: 15px;
  color: #333;
  width: 100%;
  box-sizing: border-box;
  transition: background-color 0.2s;
}

input[type='text']:focus {
  background-color: #e0e0e0;
  outline: none;
}

.coverPreview {
  max-width: 100%;
  max-height: 200px;
  border-radius: 10px;
  margin-top: 12px;
  object-fit: cover;
  border: 1px solid #ddd;
}

.buttonGroup {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  margin-top: 20px;
}

.buttonGroup .btnSave,
.buttonGroup .btnClose {
  flex: 1;
  padding: 12px 0;
  border: none;
  border-radius: var(--border-radius);
  font-family: 'Roboto', sans-serif;
  font-weight: 600;
  font-size: 15px;
  cursor: pointer;
  transition:
    background-color 0.3s,
    color 0.3s;
}

.btnSave {
  background-color: var(--accent);
  color: var(--text-light);
}

.btnSave:hover {
  background-color: var(--accent-hover);
}

.btnClose {
  background-color: #e0e0e0;
  color: var(--text-dark);
}

.btnClose:hover {
  background-color: #ccc;
}
</style>
