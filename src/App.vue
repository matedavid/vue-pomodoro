<template>
  <div class="toolbar">
    <i class="fa-solid fa-gear" @click="toggleModal"></i>
  </div>
  <div class="content-container">
    <div class="container">
      <rounded-timer :currentTimerSeconds="currentTimerSeconds" :perc="progress" />
      <timer-button
        @button-clicked="buttonClicked"
        :timerRunning="timerRunning"
      />
    </div>
  </div>
  <settings-slide-out :show="showSettings" @modal-close="toggleModal" />
</template>

<script lang="ts">
import { defineComponent } from "vue";

import TimerButton from "./components/TimerButton.vue";
import RoundedTimer from "./components/RoundedTimer.vue";
import SettingsSlideOut from "./components/SlideOutSettings.vue";

export default defineComponent({
  name: "App",
  components: {
    TimerButton,
    RoundedTimer,
    SettingsSlideOut,
  },
  data() {
    return {
      startingTimerSeconds: 0,
      currentTimerSeconds: 0,
      progress: 100,

      timerRunning: false,
      interval: -1,

      showSettings: false,
    };
  },
  created() {
    this.startingTimerSeconds = 25 * 60; // 25 minutes starting timer
    this.currentTimerSeconds = this.startingTimerSeconds;
  },
  methods: {
    decreaseSecond() {
      this.currentTimerSeconds--;
      this.progress =
        (this.currentTimerSeconds / this.startingTimerSeconds) * 100;

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
    toggleModal() {
      this.showSettings = !this.showSettings;
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

.content-container {
  width: 100%;
  display: grid;
  place-items: center;
}

.container {
  margin-top: 35px;
  padding-top: 25px;

  height: 350px;
  width: 400px;

  border-radius: 8px;
  box-shadow: 20px 20px 20px var(--black-dark);

  background-color: var(--black-dark);
  text-align: center;
}

.toolbar {
  width: 100%;
  display: grid;
  place-items: end;
}

i {
  font-size: 25px;
  padding-top: 10px;
  padding-right: 15px;
}
</style>
