<script setup lang="ts">
// const items = [
//   {
//     title: "Byron Bay The Pass",
//     value: 1,
//   },
//   {
//     title: "Byron Bay Main Beach",
//     value: 2,
//   },
//   {
//     title: "Ballina River",
//     value: 3,
//   },
// ];
</script>

<template>
  <div class="main-container rounded rounded-md">
    <div class="sidebar">
      <img src="./assets/Mark Dowton.png" alt="main-logo" class="w-full" />
      <v-list lines="three" class="text-left bg-none">
        <v-list-item
          title="Address"
          subtitle="5 Talbot Close McLeans Ridges NSW 2480"
        ></v-list-item>
        <v-list-item title="Phone" subtitle="0407 108 787"></v-list-item>
        <v-list-item
          title="Email"
          subtitle="markdowton007@gmail.com"
        ></v-list-item>
        <v-list-item
          title="LinkedIn"
          subtitle="https://www.linkedin.com/in/mark-dowton-03a85a41/"
        ></v-list-item>
      </v-list>
    </div>

    <div>
      
    </div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

.main-container {
  display: flex;
  height: 100vh;
  width: 100vw;
}
.sidebar {
  width: 360px;
  background-color: #183c67;
  color: white;
  display: flex;
  flex-direction: column;
  padding: 20px;
}

.sidebar a {
  color: white;
  text-decoration: none;
  margin-bottom: 10px;
  padding: 10px;
  transition: background 0.3s;
}

.sidebar a:hover {
  background-color: #575757;
}
.main-section {
  flex-grow: 1;
  top: 0;
  left: 0;
  background: url("./assets/marks_profile_image.png") no-repeat 260px center
    fixed;
  background-size: cover;
  position: relative;
}
.main-section::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.3); /* Black transparent overlay */
  z-index: 0;
}
.v-list {
  background: none;
  color: white;
}
</style>
