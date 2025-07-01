<script setup lang="ts">
import { ref, onUnmounted } from 'vue';
import { VueDraggableNext } from 'vue-draggable-next';

interface Item {
  id: number;
  name: string;
}

defineProps<{
  list: Item[];
  isVisible: boolean;
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

const position = ref({ x: window.innerWidth / 2 - 150, y: 100 });
const isDraggingWindow = ref(false);
const dragStartOffset = ref({ x: 0, y: 0 });
const stashboxEl = ref<HTMLElement | null>(null);

const onMouseDown = (event: MouseEvent) => {
  isDraggingWindow.value = true;
  dragStartOffset.value = {
    x: event.clientX - position.value.x,
    y: event.clientY - position.value.y,
  };
  window.addEventListener('mousemove', onMouseMove);
  window.addEventListener('mouseup', onMouseUp);
};

const onMouseMove = (event: MouseEvent) => {
  if (!isDraggingWindow.value) return;
  position.value = {
    x: event.clientX - dragStartOffset.value.x,
    y: event.clientY - dragStartOffset.value.y,
  };
};

const onMouseUp = () => {
  isDraggingWindow.value = false;
  window.removeEventListener('mousemove', onMouseMove);
  window.removeEventListener('mouseup', onMouseUp);
};

onUnmounted(() => {
  window.removeEventListener('mousemove', onMouseMove);
  window.removeEventListener('mouseup', onMouseUp);
});
</script>

<template>
  <Transition name="fade">
    <div
      v-if="isVisible"
      ref="stashboxEl"
      class="list-container floating"
      :style="{ left: `${position.x}px`, top: `${position.y}px` }"
    >
      <h2 class="drag-handle" @mousedown="onMouseDown">Stashbox</h2>
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
        <div v-for="item in list" :key="item.id" class="list-item">
          {{ item.name }}
        </div>
      </VueDraggableNext>
    </div>
  </Transition>
</template>

<style scoped>
.floating {
  position: absolute;
  z-index: 1000;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.drag-handle {
  cursor: move;
  user-select: none;
  padding: 12px 20px;
  background-color: #f7f7f7;
  border-bottom: 1px solid #e0e0e0;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
  font-size: 16px;
  font-weight: 600;
}

.list-container {
  width: 320px;
  border: none;
  padding: 0;
  border-radius: 8px;
  overflow: hidden;
}

.list {
  min-height: 200px;
  max-height: 400px;
  overflow-y: auto;
  background-color: #fff;
  padding: 10px;
}

.list-item {
  padding: 12px 15px;
  margin-bottom: 8px;
  background-color: #fff;
  border: 1px solid #e8e8e8;
  border-radius: 5px;
  cursor: grab;
  transition: background-color 0.3s;
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

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(-20px) scale(0.95);
}
</style> 