<template>
  <div class="container">
    <rounded-timer value="25" perc="50" />
    <timer-button @button-clicked="buttonClicked" text="Start" />
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
      interval: -1,
    };
  },
  created() {
    //this.interval = setInterval(this.increaseProgress, 1000);
  },
  methods: {
    increaseProgress() {
      this.progressValue--;

      if (this.progressValue == this.limitProgress) {
        clearInterval(this.interval);
      }
    },
    buttonClicked() {
      console.log(this.progressValue);
    },
  },
});
</script>

<style>
:root {
  --black-light: #3c3c3c;
  --black-dark: #292929;
  --purple-primary: #3500c4;
  --purple-secondary: #440bde;
  --red-gradient: #bd0707;
}

body {
  background-color: var(--black-light);
}

.container {
  padding-top: 25px;
  height: 350px;
  width: 400px;
  background-color: var(--black-dark);
  position: absolute;
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
  border-radius: 8px;
  box-shadow: 20px 20px 20px var(--black-dark);
  text-align: center;
}
</style>
