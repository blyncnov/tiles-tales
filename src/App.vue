<template>
  <section>
    <div class="tiles_header">
      <h2 class="selection_count">You Painted: {{ hoveredTiles.length }} tiles </h2>
      <h2 class="selection_time_left">Timer: {{ formattedTime }}</h2>
    </div>

    <div class="grid">
      <div v-for="index in 100" :key="index" class="box" :class="{
        selected: selectedTiles.includes(index - 1),
        highlight: hoveredTiles.includes(index - 1),
      }" @dblclick="handleDoubleClick(index - 1)" @mouseover="handleMouseOver(index - 1)" @click="handleClick"></div>
    </div>
  </section>
</template>

<script lang="ts">
import { ref, watch, onMounted, computed } from 'vue'

export default {
  name: "Grid",
  setup() {

    const selectedTiles = ref<number[]>([]);
    const hoveredTiles = ref<number[]>([]);
    const startTiles = ref<number | null>(null);

    const maxTime = ref(10);
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
    watch([timeLeft, hoveredTiles, selectedTiles], ([newTimeLeft, newHoveredTiles, newSelectedTiles]) => {
      if (newTimeLeft < 1) {
        alert('Time’s up! Better luck next time.');

        // RESET PAINT AND TIMER
        hoveredTiles.value = [];
        selectedTiles.value = [];
        timeLeft.value = 10;
      } else if (newTimeLeft > 1 && newHoveredTiles.length === 100 || newSelectedTiles.length === 100) {
        alert(`Incredible! You painted all the tiles in ${maxTime.value - newTimeLeft} seconds!`);

        // RESET PAINT AND TIMER
        hoveredTiles.value = [];
        selectedTiles.value = [];
        timeLeft.value = 10;
      }
    });

    const handleDoubleClick = (index: number) => {
      startTiles.value = index;
    };

    const handleMouseOver = (index: number) => {
      if (startTiles !== null) {
        if (!hoveredTiles.value.includes(index)) {
          hoveredTiles.value = [...hoveredTiles.value, index];
        }
      }
    };

    const handleClick = () => {
      if (startTiles.value !== null) {
        selectedTiles.value = [
          ...selectedTiles.value,
          ...hoveredTiles.value,
          startTiles.value
        ];
      } else {
        selectedTiles.value = [
          ...selectedTiles.value,
          ...hoveredTiles.value
        ];
      }
      startTiles.value = null
    };

    return {
      selectedTiles,
      hoveredTiles,
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
