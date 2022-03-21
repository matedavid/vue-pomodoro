<template>
  <div class="rounded-container">
    <div
      class="circular-progress"
      :style="{
        background: `conic-gradient(#4d5bf9 ${perc * 3.6}deg, #cadcff ${perc * 3.6}deg)`,
      }"
    >
      <div class="value-container">{{ value }}</div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "rounded-timer",
  props: {
    value: Number,
    perc: Number,
  }
});
</script>

<style scoped>
.rounded-container {
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
