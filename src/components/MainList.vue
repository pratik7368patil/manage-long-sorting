<script setup lang="ts">
import { VueDraggableNext } from 'vue-draggable-next';

interface Item {
  id: number;
  name: string;
}

defineProps<{
  list: Item[];
}>();

const emit = defineEmits(['drag-start', 'drag-end']);

const animateOnDrop = (event: any) => {
  event.item.classList.add('item-added');
  setTimeout(() => {
    event.item.classList.remove('item-added');
  }, 600);
};

const onDragEnd = () => {
  emit('drag-end');
};
</script>

<template>
  <div class="list-container">
    <h2>Main List</h2>
    <VueDraggableNext
      class="list"
      :list="list"
      group="items"
      :animation="300"
      @start="emit('drag-start')"
      @end="onDragEnd"
      @add="animateOnDrop"
      @update="animateOnDrop"
    >
      <div
        v-for="item in list"
        :key="item.id"
        class="list-item"
      >
        {{ item.name }}
      </div>
    </VueDraggableNext>
  </div>
</template>

<style scoped>
.list-container {
  width: 45%;
  background-color: #fff;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
  transition: box-shadow 0.3s ease;
}

.list-container:hover {
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
}

.list {
  min-height: 400px;
  padding: 10px 0;
}

.list-item {
  padding: 15px 20px;
  margin-bottom: 10px;
  background-color: #fff;
  border: 1px solid #e0e0e0;
  border-radius: 5px;
  cursor: grab;
  transition: background-color 0.3s, transform 0.3s, box-shadow 0.3s;
}

.list-item:hover {
  background-color: #f9f9f9;
}

.item-added {
  animation: drop-in 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

@keyframes drop-in {
  from {
    opacity: 0;
    transform: translateY(-30px) scale(0.9);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

:deep(.sortable-ghost) {
  background-color: #eef2f7;
  border-style: dashed;
  border-color: #a0b3c7;
}

:deep(.sortable-drag) {
  opacity: 1 !important;
  transform: rotate(3deg) scale(1.05);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.15);
}
</style> 