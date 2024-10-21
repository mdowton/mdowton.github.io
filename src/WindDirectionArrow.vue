<template>
  <div class="arrow-container" :style="{ transform: `rotate(${rotation}deg)` }">
    <v-icon class="wind-arrow">mdi-arrow-up</v-icon> <!-- Use any arrow icon -->
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

const props = defineProps({
  windDirection: {
    type: String,
    required: true,
    default: '0'
  },
});

// Map wind directions to rotation angles
const directionToDegrees:any = {
  S: 0,
  SSW: 22.5,
  SW: 45,
  WSW: 67.5,
  W: 90,
  WNW: 112.5,
  NW: 135,
  NNW: 157.5,
  N: 180,
  NNE: 202.5,
  NE: 225,
  ENE: 247.5,
  E: 270,
  ESE: 292.5,
  SE: 315,
  SSE: 337.5,
};

// Compute rotation angle based on wind direction
const rotation = computed(() => {
  return directionToDegrees[props.windDirection] || 0; // Default to 0 for undefined directions
});
</script>

<style scoped>
.arrow-container {
  display: flex;
  justify-content: center;
  align-items: center;
  transition: transform 0.5s ease; /* Smooth rotation */
}
.wind-arrow {
  font-size: 48px; /* Adjust size as needed */
}
</style>
