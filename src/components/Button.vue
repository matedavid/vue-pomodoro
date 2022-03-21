<template>
  <button>{{ text }}</button>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "own-button",
  components: {},
  props: {
    text: String
  }

});
</script>

<style scoped>

</style>