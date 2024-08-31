<script setup lang="ts">
import { ref } from 'vue'

defineProps<{ msg: string }>()

const count = ref(0)
</script>

<template>
  <div v-for="index in 100" :key="index" class="box" :class="{
    selected: selectedBoxes.includes(index - 1),
    highlight: hoveredBoxes.includes(index - 1),
  }" @dblclick="handleDoubleClick(index - 1)" @mouseover="handleMouseOver(index - 1)" @click="handleClick"></div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
