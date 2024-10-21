<template>
    <v-container>
      <v-row>
        <v-col cols="12">
            <v-card-subtitle v-if="nextTide">
            <v-icon icon="mdi-moon-new"></v-icon><span class="capitalize"> Next tide: {{ nextTide.type }} at {{ nextTide.time }} with height {{ nextTide.height }} m.</span>
          </v-card-subtitle>
          <Line :data="chartData" /> 
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script>
  import { ref, onMounted, computed } from 'vue';
  import { Line } from 'vue-chartjs';
  import { Chart as ChartJS, CategoryScale, LinearScale, PointElement, LineElement } from 'chart.js'; // Import necessary Chart.js components
  import tides from './tide-data/tides-2024';
  
  // Register Chart.js components
  ChartJS.register(CategoryScale, LinearScale, PointElement, LineElement);
  
  export default {
    name: 'TideData',
    components: {
      Line,
    },
    setup() {
      const tideHeights = ref([]);
      const tideTimes = ref([]);
      const nextTide = ref(null);

  
      const today = new Date();
      const month = today.toLocaleString('default', { month: 'long' });
      const day = today.toISOString().slice(0, 10);
      const currentTime = today.toTimeString().slice(0, 5).replace(':', ''); // Get current time in HHMM format
      
      onMounted(() => {
      const monthData = tides[month];
      if (monthData) {
        const todayData = monthData.find(entry => entry.date === day);

        if (todayData) {
          tideHeights.value = todayData.tides.map(t => t.height);
          tideTimes.value = todayData.tides.map(t => t.time);

          // Determine the next tide
          for (const tide of todayData.tides) {
            if (tide.time > currentTime) {
              nextTide.value = tide;
              break;
            }
          }
        }
      }
    });
      const chartData = computed(() => {
        const monthData = tides[month];
        if (monthData) {
          const todayData = monthData.find(entry => entry.date === day);
          if (todayData) {
            tideHeights.value = todayData.tides.map(t => t.height);
            tideTimes.value = todayData.tides.map(t => t.time);
          }
        }
        return {
          labels: tideTimes.value,
          datasets: [
            {
              label: 'Tide Heights (m)',
              data: tideHeights.value,
              borderColor: '#42a5f5',
              fill: true,
              tension: 0.4,
            },
          ],
        };
      });
      return {
        chartData,
        tideHeights,
        tideTimes,
        nextTide
      };
    },
  };
  </script>
  
  <style scoped>
  /* Custom styles if needed */
  </style>
  