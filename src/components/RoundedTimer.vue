<template>
  <div class="rounded-container">
    <div
      class="circular-progress"
      :style="{
        background: `conic-gradient(var(--purple-secondary) ${perc * 3.6}deg, var(--black-light) ${perc * 3.6}deg)`,
      }"
    >
      <div class="value-container">
        {{ currentTimerAsDisplayText(currentTimerSeconds) }}
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

function currentTimerAsDisplayText(seconds: number): string {
  let minutes = Math.floor(seconds / 60);
  let secs = seconds - minutes * 60;

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
  name: "rounded-timer",
  props: {
    currentTimerSeconds: Number,
    perc: Number,
  },
  methods: {
    currentTimerAsDisplayText,
  },
});
</script>

<style scoped>
.rounded-container {
  display: grid;
  place-items: center;
}

.circular-progress {
  position: relative;
  height: 230px;
  width: 230px;

  border-radius: 50%;

  display: grid;
  place-items: center;
}

.circular-progress:before {
  content: "";
  position: absolute;
  height: 88%;
  width: 88%;
  background-color: var(--black-dark);
  border-radius: 50%;
}

.value-container {
  position: relative;
  font-family: Arial;
  font-size: 50px;
  color: white;
}
</style>
