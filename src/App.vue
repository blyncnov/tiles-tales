<template>
  <div>
    <div class="tiles_header">
      <h2 class="selection_count">You Painted: {{ hoveredBoxes.length }} tiles </h2>
      <h2 class="selection_time_left">Timer: {{ formattedTime }}</h2>
    </div>

    <div class="grid">
      <div v-for="index in 100" :key="index" class="box" :class="{
        selected: selectedTiles.includes(index - 1),
        highlight: hoveredBoxes.includes(index - 1),
      }" @dblclick="handleDoubleClick(index - 1)" @mouseover="handleMouseOver(index - 1)" @click="handleClick"></div>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, watch, onMounted, computed } from 'vue'

export default {
  name: "Grid",
  setup() {

    const selectedTiles = ref([]);
    const hoveredBoxes = ref([]);
    const startTiles = ref(null);

    const timeLeft = ref(10);
    const timer = ref<ReturnType<typeof setInterval> | null>(null);

    // Format the time as MM:SS
    const formattedTime = computed(() => {
      const minutes = Math.floor(timeLeft.value / 60);
      const seconds = timeLeft.value % 60;
      return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
    });

    // Start the timer
    const startTimer = () => {
      if (timer.value !== null) return;

      timer.value = setInterval(() => {
        if (timeLeft.value > 0) {
          timeLeft.value--;
        } else {
          clearInterval(timer.value!);
          timer.value = null;
        }
      }, 1000);
    };

    // When Page loads
    onMounted(() => {
      startTimer()
    })

    // Watch Timer
    watch([timeLeft, hoveredBoxes], ([newTimeLeft, newHoveredBoxes]) => {
      if (newTimeLeft < 1) {
        alert('Time’s up! Better luck next time.');

        // RESET PAINT AND TIMER
        hoveredBoxes.value = [];
        timeLeft.value = 10;
      } else if (newTimeLeft > 1 && newHoveredBoxes.length === 100) {
        alert(`Incredible! You painted all the tiles with ${newTimeLeft} seconds remaining!`);

        // RESET PAINT AND TIMER
        hoveredBoxes.value = [];
        timeLeft.value = 10;
      }
    });


    const handleDoubleClick = (index: number) => {
      startTiles.value = index;
    };

    const handleMouseOver = (index: number) => {
      if (startTiles !== null) {
        if (!hoveredBoxes.value.includes(index)) {
          hoveredBoxes.value = [...hoveredBoxes.value, index];
        }
      }
    };

    const handleClick = () => {
      selectedTiles.value = [...selectedTiles, ...hoveredBoxes, startTiles];
      hoveredBoxes.value = [];
      startTiles.value = null
    };

    return {
      selectedTiles,
      hoveredBoxes,
      handleDoubleClick,
      handleMouseOver,
      handleClick,
      timeLeft,
      formattedTime,
      startTimer,
    };
  },
};
</script>


<style scoped>
.tiles_header {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.selection_count,
.selection_time_left {
  font-size: 18px;
  color: white !important;
}
</style>
