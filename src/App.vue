<script setup lang="ts">
import { ref, computed } from 'vue';
import MainList from './components/MainList.vue';
import Stashbox from './components/Stashbox.vue';

interface Item {
  id: number;
  name: string;
}

const mainList = ref<Item[]>(
  Array.from({ length: 100 }, (_, i) => ({ 
    id: i + 1, 
    name: `Item ${i + 1}` 
  }))
);

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
