<script setup lang="ts">

import Modal from '@/components/Modal.vue'
import { provide, ref } from 'vue'
import FolderTree from '@/components/FolderTree.vue'

const isModalOpen = ref(false)

const folders = ref([
  {
    id: 1,
    name: 'Папка 1',
    children: [
      { id: 2, name: 'Папка 1.1', children: [] },
      { id: 3, name: 'Папка 1.2', children: [
          { id: 4, name: 'Папка 1.2.1', children: [] }
        ]}
    ]
  },
  { id: 5, name: 'Папка 2', children: [] },
]);

const openModal = () => {
  isModalOpen.value = true
}

const closeModal = () => {
  isModalOpen.value = false
}

const selectedFolderIdTemp = ref(-1)
const selectedFolderId = ref(-1)

const selectFolderIdTemp = (id: number) => {
  if (selectedFolderIdTemp.value !== id) {
    selectedFolderIdTemp.value = id
  } else {
    selectedFolderIdTemp.value = -1
  }
}

const confirmSelected = () => {
  selectedFolderId.value = selectedFolderIdTemp.value
  closeModal()
}

const getFolderNameById = (id: number) => {
  const searchFolder = (folders: Array<string>) => {
    for (const folder of folders) {
      if (folder.id === id) {
        return folder.name
      }
      if (folder.children.length > 0) {
        const found = searchFolder(folder.children)
        if (found) return found
      }
    }
    return null
  }
  return searchFolder(folders.value)
}

provide('folderTree', {
  selectedFolderIdTemp,
  selectFolderIdTemp
})

</script>

<template>
  <div style="display: flex; flex-direction: column; gap: 10px">
    <h2 v-if="selectedFolderId == -1">Нажмите кнопку "Открыть"</h2>
    <button @click="openModal" style="margin: 0 auto">Открыть</button>
  </div>
  <Modal v-if="isModalOpen" @closeModal="closeModal" @confirmSelected="confirmSelected">
    <folder-tree :folders="folders"></folder-tree>
  </Modal>
  <div style="text-align: center; padding-top: 50px;">
    <h3 v-if="selectedFolderId == -1">{{ "Нет выбранных папок" }}</h3>
    <h3 v-else>{{ "Вы выбрали папку:" }}</h3>
    <p v-if="selectedFolderId !== -1">{{ getFolderNameById(selectedFolderId) }} (<span style="color: green">ID:</span> {{ selectedFolderId }})</p>
  </div>
</template>
