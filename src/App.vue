<template>
  <div class="toolbar">
    <i class="fa-solid fa-gear settings-icon" @click="toggleModal"></i>
  </div>
  <div class="content-container">
    <div class="container">
      <current-state :selection="currentState" />
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
    :currentStartingTime="startingTimerSeconds / 60"
    :currentRestTime="5"
    :currentLongRestTime="15"
    @settings-close="toggleModal"
    @settings-save="updateSettings"
  />
</template>

<script lang="ts">
import { defineComponent } from "vue";

import TimerButton from "./components/TimerButton.vue";
import RoundedTimer from "./components/RoundedTimer.vue";
import SettingsSlideOut from "./components/SlideOutSettings.vue";
import CurrentState from "./components/CurrentState.vue"

enum TimerState {
  Pomodoro,
  ShortBreak,
  LongBreak,
}

export default defineComponent({
  name: "App",
  components: {
    TimerButton,
    RoundedTimer,
    SettingsSlideOut,
    CurrentState
  },
  data() {
    return {
      startingTimerSeconds: 25 * 60, // 25 minutes
      shortRestTimeSeconds: 5 * 60, // 5 minutes
      longRestTimeSeconds: 15 * 60, // 15 minutes
      shortBreaksUntilLong: 3,

      currentTimerSeconds: 25 * 60, // same as startingTimerSeconds
      progress: 100,

      currentState: TimerState.Pomodoro,
      numberShortBreaks: 0,

      timerRunning: false,
      interval: -1,

      showSettings: false,
    };
  },
  methods: {
    decreaseSecond() {
      this.currentTimerSeconds--;

      if (this.currentTimerSeconds < 0) {
        alert(
          `${
            this.currentState == TimerState.Pomodoro
              ? "Pomodoro"
              : this.currentState == TimerState.ShortBreak
              ? "Short Break"
              : "Long Break"
          } finished`
        );
        clearInterval(this.interval);

        this.nextState();
      }

      let dividend =
        this.currentState == TimerState.Pomodoro
          ? this.startingTimerSeconds
          : this.currentState == TimerState.ShortBreak
          ? this.shortRestTimeSeconds
          : this.longRestTimeSeconds; // TimerState.LongBreak

      this.progress = (this.currentTimerSeconds / dividend) * 100;
    },
    nextState() {
      if (
        this.currentState == TimerState.Pomodoro &&
        this.numberShortBreaks < this.shortBreaksUntilLong
      ) {
        this.currentState = TimerState.ShortBreak;
        ++this.numberShortBreaks;
      } else if (
        this.currentState == TimerState.Pomodoro &&
        this.numberShortBreaks >= this.shortBreaksUntilLong
      ) {
        this.currentState = TimerState.LongBreak;
        this.numberShortBreaks = 0;
      } else if (
        this.currentState == TimerState.ShortBreak ||
        this.currentState == TimerState.LongBreak
      ) {
        this.currentState = TimerState.Pomodoro;
      }

      this.currentTimerSeconds =
        this.currentState == TimerState.Pomodoro
          ? this.startingTimerSeconds
          : this.currentState == TimerState.ShortBreak
          ? this.shortRestTimeSeconds
          : this.longRestTimeSeconds; // TimerState.LongBreak

      this.timerRunning = false;
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

  width: 400px;

  border-radius: 8px;
  box-shadow: 20px 20px 20px var(--black-dark);

  background-color: var(--black-dark);
  text-align: center;
  padding-bottom: 20px;
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
