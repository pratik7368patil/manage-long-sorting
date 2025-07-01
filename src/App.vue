<script setup lang="ts">
import { ref, computed } from 'vue';
import MainList from './components/MainList.vue';
import Stashbox from './components/Stashbox.vue';

interface Item {
  id: number;
  name: string;
}

const mainList = ref<Item[]>([
  { id: 1, name: 'Item 1' },
  { id: 2, name: 'Item 2' },
  { id: 3, name: 'Item 3' },
  { id: 4, name: 'Item 4' },
  { id: 5, name: 'Item 5' },
  { id: 6, name: 'Item 6' },
  { id: 7, name: 'Item 7' },
  { id: 8, name: 'Item 8' },
  { id: 9, name: 'Item 9' },
  { id: 10, name: 'Item 10' },
]);

const stashboxList = ref<Item[]>([]);
const isDragging = ref(false);

const isStashboxVisible = computed(() => {
  return isDragging.value || stashboxList.value.length > 0;
});

const handleDragStart = () => {
  isDragging.value = true;
};

const handleDragEnd = () => {
  isDragging.value = false;
};
</script>

<template>
  <div class="app-container">
    <MainList
      :list="mainList"
      @drag-start="handleDragStart"
      @drag-end="handleDragEnd"
    />
    <Stashbox
      :list="stashboxList"
      :is-visible="isStashboxVisible"
      @drag-start="handleDragStart"
      @drag-end="handleDragEnd"
    />
  </div>
</template>

<style scoped>
.app-container {
  display: flex;
  justify-content: space-around;
  width: 100%;
  max-width: 1200px;
}
</style>
