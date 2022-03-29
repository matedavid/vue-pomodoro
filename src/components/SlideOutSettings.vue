<template>
  <div class="slideout" :class="show ? 'on' : ''">
    <div class="header">
      <h1 class="settings-title">Settings</h1>
      <div class="exit-icon">
        <i class="fa-solid fa-xmark" @click="$emit('modal-close')"></i>
      </div>
    </div>
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

.header {
  display: grid;
  grid-template-areas: "title icon";
}

.settings-title {
  color: white;
  margin-left: 15px;
  grid-area: title;
}
.exit-icon {
  margin-top: 22px;
  grid-area: icon;
  color: var(--purple-secondary);
  text-align: end;
}
</style>