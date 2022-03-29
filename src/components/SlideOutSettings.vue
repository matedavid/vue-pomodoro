<template>
  <div class="slideout" :class="show ? 'on' : ''">
    <h1 class="settings_title">Settings</h1>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "settings-slideout",
  props: {
    currentStartingTime: Number,
    currentRestTime: Number,
    show: Boolean,
  },
  emits: ["modal-close", "modal-save"],
});
</script>

<style scoped>
.slideout {
  background: var(--black-dark);
  color: #333;
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

.settings_title {
  color: white;
  margin-left: 15px;
}
</style>