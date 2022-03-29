<template>
  <div class="toolbar">
    <i class="fa-solid fa-gear settings-icon" @click="toggleModal"></i>
  </div>
  <div class="content-container">
    <div class="container">
      <rounded-timer
        :currentTimerSeconds="currentTimerSeconds"
        :perc="progress"
      />
      <timer-button
        @button-clicked="buttonClicked"
        :timerRunning="timerRunning"
      />
    </div>
  </div>
  <settings-slide-out
    :show="showSettings"
    @settings-close="toggleModal"
    @settings-save="updateSettings"
    :currentStartingTime="startingTimerSeconds / 60"
    :currentRestTime="5"
    :currentLongRestTime="15"
  />
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
      startingTimerSeconds: 25 * 60, // 25 minutes
      shortRestTimeSeconds: 5 * 60, // 5 minutes
      longRestTimeSeconds: 15 * 60, // 15 minutes

      currentTimerSeconds: 25 * 60, // same as startingTimer
      progress: 100,

      timerRunning: false,
      interval: -1,

      showSettings: false,
    };
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
    updateSettings(data: {
      pomodoroTime: number;
      shortRestTime: number;
      longRestTime: number;
    }) {
      if (this.timerRunning) {
        alert("Can't change configuration while the timer is running");
        return;
      }

      this.startingTimerSeconds = data.pomodoroTime * 60;
      this.shortRestTimeSeconds = data.shortRestTime * 60;
      this.longRestTimeSeconds = data.longRestTime * 60;
      this.currentTimerSeconds = this.startingTimerSeconds;
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

  --settings-input-font-size: 20px;
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

.settings-icon {
  font-size: 25px;
  padding-top: 10px;
  padding-right: 15px;
}
</style>
