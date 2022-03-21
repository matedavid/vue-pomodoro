<template>
  <button @click="$emit('button-clicked')">{{ !timerRunning ? "Start" : "Stop" }}</button>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "timer-button",
  components: {},
  props: {
    timerRunning: Boolean,
  },
});
</script>

<style scoped>
button {
  margin-top: 35px;
  font-size: 25px;

  background-color: var(--purple-primary); /* Green */
  border: none;
  color: white;
  padding: 15px 50px;
  border-radius: 5%;
}

button:hover {
  box-shadow: 0 5px 10px var(--purple-secondary);
}

</style>