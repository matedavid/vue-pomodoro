<template>
  <div class="slideout" :class="show ? 'on' : ''">
    <header>
      <h1 class="settings-title">Settings</h1>
      <div class="exit-icon">
        <i class="fa-solid fa-xmark" @click="$emit('settings-close')"></i>
      </div>
    </header>

    <div class="content">
      <h3>Times (in minutes)</h3>
      <div class="times">
        <p class="time-pomodoro-label">Pomodoro</p>
        <p class="time-short-label">Short Break</p>
        <p class="time-long-label">Long Break</p>

        <input class="time-pomodoro" type="number" v-model="pomodoroTime" />
        <input class="time-short" type="number" v-model="restTime" />
        <input class="time-long" type="number" v-model="longRestTime" />
      </div>
    </div>

    <footer>
      <button
        @click="
          $emit('settings-save', {
            pomodoroTime: pomodoroTime,
            shortRestTime: restTime,
            longRestTime: longRestTime,
          })
        "
      >
        Save
      </button>
    </footer>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "settings-slideout",
  props: {
    currentStartingTime: Number,
    currentRestTime: Number,
    currentLongRestTime: Number,

    show: Boolean,
  },
  data() {
    return {
      pomodoroTime: this.currentStartingTime,
      restTime: this.currentRestTime,
      longRestTime: this.currentLongRestTime,
    };
  },
  emits: ["settings-close", "settings-save"],
});
</script>

<style scoped>
.slideout {
  background-color: var(--black-dark);
  color: white;

  position: fixed;
  top: 0;
  right: -100%;
  width: 30%;
  height: 100%;

  -webkit-transition-duration: 0.3s;
  -moz-transition-duration: 0.3s;
  -o-transition-duration: 0.3s;
  transition-duration: 0.3s;
}

.slideout.on {
  right: 0;
}

header {
  display: grid;
  grid-template-areas: "title icon";
}

.settings-title {
  grid-area: title;

  color: white;
  margin-left: 15px;
}
.exit-icon {
  grid-area: icon;

  font-size: 25px;
  margin-top: 22px;
  margin-right: 15px;
  color: var(--purple-secondary);
  text-align: end;
}

.content {
  margin-left: 15px;
}

.times {
  display: grid;
  grid-template-areas:
    "pomodoro-label short-label long-label"
    "pomodoro-input short-input long-input";
}

.time-pomodoro-label {
  grid-area: pomodoro-label;
}

.time-short-label {
  grid-area: short-label;
}

.time-long-label {
  grid-area: long-label;
}

.time-pomodoro {
  grid-area: pomodoro-input;
  width: 60px;
  height: 30px;
  font-size: var(--settings-input-font-size);
}

.time-short {
  grid-area: short-input;
  width: 60px;
  height: 30px;
  font-size: var(--settings-input-font-size);
}

.time-long {
  grid-area: long-input;
  width: 60px;
  height: 30px;
  font-size: var(--settings-input-font-size);
}

footer {
  margin-top: 20px;
  margin-left: 15px;
}
</style>