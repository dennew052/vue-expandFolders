<script setup lang="ts">

import { inject } from 'vue'

const { selectedFolderIdTemp, selectFolderIdTemp } = inject('folderTree')

const props = defineProps({
  folders: Array<string>
})

const toggleFolder = (folder: any) => {
  folder.open = !folder.open;
}

</script>

<template>
  <ul>
    <li v-for="folder in folders" :key="folder.id">
      <span
        v-if="folder.children.length > 0"
        class="arrow"
        @click="toggleFolder(folder)"
      >
          {{ folder.open ? '▼' : '▶' }}
        </span>

      <span
        @click="selectFolderIdTemp(folder.id)"
        :class="{ selected: selectedFolderIdTemp == folder.id }"

      >{{ folder.name }}</span>

      <folder-tree v-if="folder.children.length && folder.open" :folders="folder.children"></folder-tree>

    </li>
  </ul>
</template>

<style scoped>
.selected {
  background-color: #d3d3d3;
  padding: 2px 5px;
  border-radius: 3px;
}

.arrow {
  cursor: pointer;
  margin-right: 10px;
}

li {
  list-style: none;
}
</style>
