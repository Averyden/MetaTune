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

<style>
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
  background: #fff;
  border-radius: 10px;
  height: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  animation: slideDown 0.3s ease forwards;
  padding: 20px;
  width: 90%;
  max-width: 400px;
}

.modalContent.closing {
  animation: slideUp 0.3s ease forwards;
}

.modalContent h2 {
  font-family: 'Roboto', sans-serif;
  font-weight: bold;
  font-size: 25px;
  text-align: center;
  padding: 10px;
  color: black;
}

label {
  font-family: 'Roboto', sans-serif;
  color: #2e2e2e;
  display: block;
  text-align: left;
  font-weight: bold;
  margin: 10px 0 5px;
}

input {
  font-family: 'Roboto', sans-serif;
  color: #2e2e2e;
  background-color: #b0b0b0;
  font-size: 15px;
  padding: 16px;
  text-align: center;
  border-radius: 15px;
  width: 70%;
  height: 7%;
  resize: none;
  border: none;
  overflow: hidden;
}

.buttonGroup {
  display: flex;
  justify-content: space-between;
  padding-top: 15px;
}

.buttonGroup .btnSave,
.buttonGroup .btnClose {
  margin: 8px;
  min-width: 75px;
  width: auto;
  height: 30px;
  border: none;
  border-radius: 15px;
  font-family: 'Roboto', sans-serif;
  font-size: auto;
  transition: background 0.3s;
  cursor: pointer;
}

.btnSave {
  background-color: #f13ce8;
  color: #fff;
  transition: background 0.3s;
}

.btnSave:hover {
  background: #d823cf;
}

.coverPreview {
  max-width: 100%;
  max-height: 200px;
  border-radius: 10px;
  margin-top: 10px;
}
</style>
