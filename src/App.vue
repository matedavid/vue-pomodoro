<template>
  <!-- <div class="container">
    <RoundedTimer />
    <Button text="Start" />
  </div> -->

  <div class="container">
    <div
      class="circular-progress"
      :style="{
        background: `conic-gradient(#4d5bf9 ${progressValue * 3.6}deg, #cadcff ${progressValue * 3.6}deg)`,
      }"
    >
      <div class="value-container">{{ progressValue }}%</div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

import Button from "./components/Button.vue";
import RoundedTimer from "./components/RoundedTimer.vue";

export default defineComponent({
  name: "App",
  components: {
    //Button,
    //RoundedTimer,
  },
  data() {
    return {
      progressValue: 100,
    };
  },
  methods: {
    increaseProgress() {
      this.progressValue--;
    },
  },
  created() {
    setInterval(this.increaseProgress, 1000);
  },
});
</script>

<style>
body {
  height: 100vh;
}

.container {
  height: 400px;
  width: 400px;
  background-color: white;
  position: absolute;
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
  border-radius: 8px;

  box-shadow: 20px 20px 40px rgba(60, 60, 150, 0.25);

  display: grid;
  place-items: center;
}

.circular-progress {
  position: relative;
  height: 250px;
  width: 250px;
  background-color: orange;

  border-radius: 50%;

  display: grid;
  place-items: center;
}

.circular-progress:before {
  content: "";
  position: absolute;
  height: 84%;
  width: 84%;
  background-color: white;
  border-radius: 50%;
}

.value-container {
  position: relative;
  font-family: Arial;
  font-size: 50px;
}
</style>
