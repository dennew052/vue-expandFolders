<script setup lang="ts">
const emit = defineEmits(['closeModal', 'confirmSelected'])

</script>

<template>
  <div class="modal-overlay">
    <div class="modal-content">
      <h2>Выделите нужную папку и нажмите "ОК"</h2>
      <div style="padding:40px 0">
        <slot></slot>
      </div>
      <div class="modal-footer">
        <button @click="emit('closeModal');">Закрыть</button>
        <button @click="emit('confirmSelected')">ОК</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
}

.modal-footer {
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
}
</style>
