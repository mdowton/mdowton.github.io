<script setup lang="ts">
import { ref } from 'vue';
import WindDirectionArrow from './WindDirectionArrow.vue';
import TideData from './TideData.vue';
interface WeatherData {
  location: {
    name: string;
  };
  current: {
    temp_c: null,
    wind_kph: null,
    wind_dir: 'N',
    gust_kph: null,
    condition: {
      text: null,
    }
  }
}
const items = ref([
  {
    title: "Byron Bay The Pass",
    value: 1,
    icon: 'mdi-waves',
    videoUrl: "https://content.jwplatform.com/previews/SQKVRAYZ",
    lat: -28.65,
    long: 153.617,
  },
  {
    title: "Byron Bay Main Beach",
    value: 2,
    icon: 'mdi-waves',
    videoUrl: "https://content.jwplatform.com/previews/X97v2Xj2",
    lat: -28.65,
    long: 153.617,
  },
  {
    title: "Ballina River",
    value: 3,
    icon: 'mdi-waves',
    videoUrl: "https://widget.coastalcoms.com/video/76818de6-1bc6-44d2-943a-3a905ee9c132",
    lat: -28.89,
    long: 153.54,
  },
]);

// Define reactive properties to hold the selected video URL and title
const videoUrl = ref("");
const videoTitle = ref("");
const weatherData = ref<WeatherData | null>(null); // Initially set to null
const apiKey = "9ca648f59e3c4a72bd8233743242010"; 
// const stormGlassApi = "227a2d2c-8f57-11ef-9159-0242ac130003-227a2d90-8f57-11ef-9159-0242ac130003"

const fetchWeatherData = async (pos: any) => {
  try {
    weatherData.value = null;
    const {lat, long} = pos;
    const response = await fetch(`https://api.weatherapi.com/v1/current.json?q=${lat},${long}&key=${apiKey}`);
    const data = await response.json();
    weatherData.value = data;
  } catch (error) {
    console.error("Error fetching weather data:", error);
  }
};

const handleItemClick = (item: { videoUrl: string; title: string; }) => {
  videoUrl.value = item.videoUrl;
  videoTitle.value = item.title;
  fetchWeatherData(item);
};

</script>

<template>
  <div class="main-container">
    <!-- Sidebar for the surf cam list -->
    <div class="sidebar">
      <img src="./assets/Mark Dowton.png" alt="main-logo" class="logo" />
    
      <v-list lines="three" class="surf-list bg-transparent">
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :value="item"
          color="primary"
          rounded="xl"
          @click="handleItemClick(item)"
          class="list-item"
        >
          <template v-slot:prepend>
            <v-icon :icon="item.icon" class="mt-2"></v-icon>
          </template>
          <v-list-item-title v-text="item.title"></v-list-item-title>
        </v-list-item>
      </v-list>
    </div>
    
    <!-- Title and video embed section -->
    <div class="video-container flex flex-col justify-center">
      <!-- Display weather data if available -->
      <div v-if="weatherData" class=" mb-3 flex">
        <v-card class="weather-card mr-3 w-40" elevation="2">
          <v-card-title >{{ weatherData.location.name }}</v-card-title>
          <v-card-subtitle >
            <p>Condition: {{ weatherData.current.condition.text }}</p>
          </v-card-subtitle>
          <v-card-subtitle class="mt-2">
            <p><v-icon icon="mdi-weather-sunny" class="mb-2 mr-1"></v-icon><span class="text-h4 font-weight-black"> {{ weatherData.current.temp_c }} °C</span></p>
          </v-card-subtitle>
        </v-card>
        <v-card class="weather-card mr-3 w-40" elevation="2">
          <v-card-title >Wind speed</v-card-title>
          <v-card-subtitle >
            <p>
              <v-icon icon="mdi-weather-windy" color="success" class="mb-2 mr-1"></v-icon><span class="text-h4 font-weight-black"> {{ weatherData.current.wind_kph }} kph {{ weatherData.current.wind_dir}}</span>
            </p>
          </v-card-subtitle> 
          <v-card-subtitle >
            <v-icon icon="mdi-empty"></v-icon> {{ weatherData.current.gust_kph }} kph gust
          </v-card-subtitle>
          <WindDirectionArrow :windDirection="weatherData.current.wind_dir" />
        </v-card>
        <v-card class="weather-card w-40" elevation="2">
          <v-card-title >Tides</v-card-title>
          <TideData />
        </v-card>
      </div>
      <div class="flex justify-center mt-3">
        <transition name="fade">
          <iframe
            v-if="videoUrl"
            class="video-frame"
            :src="videoUrl"
            frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
            allowfullscreen
          ></iframe>
      </transition>
      </div>
      
    </div>
  </div>
</template>

<style scoped>
body {
  color: #444444;
}

.main-container {
  display: flex;
  height: 100vh;
  width: 100vw;
  overflow: hidden;
}

.sidebar {
  width: 300px;
  background-color: #183c67;
  color: white;
  display: flex;
  flex-direction: column;
  padding: 20px;
  overflow-y: auto;
}
.v-list-item--density-default.v-list-item--three-line .v-list-item__prepend {
  padding-top:15px;
}
.logo {
  max-width: 100%;
  margin-bottom: 20px;
}

.surf-list {
  color: #2f2f2f; /* Darker text color for readability */
}

.list-item {
  transition: background-color 0.3s, color 0.3s;
  cursor: pointer;
}

.list-item:hover {
  background-color: #ffffff30; /* Subtle hover effect */
  color: #ffffff;
}

.video-container {
  flex-grow: 1;
  display: flex;
  flex-direction: column; /* Stack title and video vertically */
  justify-content: center;
  padding: 20px;
  transition: opacity 0.5s ease;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0;
}

iframe {
  border-radius: 8px;
  width: 80%; /* Responsive width */
  height: auto; /* Maintain aspect ratio */
}

.video-title {
  font-size: 24px;
  text-align: center;
  color: #333; /* Darker color for the title */
  margin-bottom: 10px; /* Space between title and video */
}
.video-frame {
  border-radius: 8px;
  width: 640px;  /* Set to 80% width for responsiveness */
  height:360px; /* Fixed height to maintain aspect ratio */
}
.weather-info {
  width: 50%;
}
.text-primary {
  color: white;
}
.w-40 {
  width: 40% !important
}
</style>
