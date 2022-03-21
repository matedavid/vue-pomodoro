<template>
  <button>{{ text }}</button>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "timer-button",
  components: {},
  props: {
    text: String
  }

});
</script>

<style scoped>
button {
  margin-top: 25px; 
  font-size: 25px;
}
</style>