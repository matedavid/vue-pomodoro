<template>
  <div class="container">
    <rounded-timer value=25 perc=50 />
    <timer-button text="Start" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

import TimerButton from "./components/TimerButton.vue";
import RoundedTimer from "./components/RoundedTimer.vue";

export default defineComponent({
  name: "App",
  components: {
    TimerButton,
    RoundedTimer,
  },
  data() {
    return {
      progressValue: 10,
      limitProgress: 0,
      interval: -1
    };
  },
  methods: {
    increaseProgress() {
      this.progressValue--;

      if (this.progressValue == this.limitProgress) {
        clearInterval(this.interval);
      }
    },
  },
  created() {
    this.interval = setInterval(this.increaseProgress, 1000);
  },
});
</script>

<style>
.container {
  padding-top: 25px;
  height: 350px;
  width: 400px;
  background-color: white;
  position: absolute;
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
  border-radius: 8px;
  box-shadow: 20px 20px 40px rgba(60, 60, 150, 0.25);

  text-align: center;
}

</style>
