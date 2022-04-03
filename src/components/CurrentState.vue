<template>
  <div class="current-state">
    <div class="state" :class="selection == 0 ? 'selected' : ''">Pomodoro</div>
    <div class="state" :class="selection == 1 ? 'selected' : ''">Short Break</div>
    <div class="state" :class="selection == 2 ? 'selected' : ''">Long Break</div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "current-state",
  props: {
    selection: Number,
  }
});
</script>

<style scoped>
.current-state {
  display: flex;
  justify-content: space-between;
  padding-right: 20px;
  padding-left: 20px;
  padding-bottom: 20px;
  font-size: 16px;
}

.state {
  padding: 8px;
  border-radius: 10px;
  color: white;
}

.selected {
  background-color: var(--black-light);
  font-weight: bold;
}
</style>