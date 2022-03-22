<template>
  <div class="container">
    <rounded-timer :currentTimer="timerTextDisplay" :perc="progress" />
    <timer-button @button-clicked="buttonClicked" :timerRunning="timerRunning" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

import TimerButton from "./components/TimerButton.vue";
import RoundedTimer from "./components/RoundedTimer.vue";

function currentTimerAsDisplayText(seconds: number): string {
  let minutes = Math.floor(seconds / 60);
  let secs = seconds - minutes*60;

  let displayString = "";
  if (minutes < 10) {
    displayString += "0";
  }
  displayString += `${minutes}:`;

  if (secs < 10) {
    displayString += "0";
  }
  displayString += `${secs}`;

  return displayString;
}

export default defineComponent({
  name: "App",
  components: {
    TimerButton,
    RoundedTimer,
  },
  data() {
    return {
      startingTimerSeconds: 0,
      currentTimerSeconds: 0,
      timerTextDisplay: "",
      progress: 100,

      timerRunning: false,
      interval: -1,
    };
  },
  created() {
    this.startingTimerSeconds = 25*60; // 25 minutes starting timer
    this.currentTimerSeconds = this.startingTimerSeconds;
    this.timerTextDisplay = currentTimerAsDisplayText(this.currentTimerSeconds);
  },
  methods: {
    decreaseSecond() {
      this.currentTimerSeconds--;
      this.progress = this.currentTimerSeconds/this.startingTimerSeconds * 100;

      this.timerTextDisplay = currentTimerAsDisplayText(this.currentTimerSeconds);

      if (this.currentTimerSeconds == 0) {
        console.log("Timer finished");
        clearInterval(this.interval);
      }
    },
    buttonClicked() {
      if (!this.timerRunning) {
        this.interval = setInterval(this.decreaseSecond, 1000);
        this.timerRunning = true;
      } else if (confirm("You sure you want to stop it?")) {
        clearInterval(this.interval);
        this.timerRunning = false;
      }
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
  display: grid;
  place-items: center;
}

.container {
  margin-top: 50px;
  padding-top: 25px;

  height: 350px;
  width: 400px;

  border-radius: 8px;
  box-shadow: 20px 20px 20px var(--black-dark);

  background-color: var(--black-dark);
  text-align: center;
}
</style>
