<template>
  <MessageWindow />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import MessageWindow from "@/components/MessageWindow.vue";

export default defineComponent({
  name: "HomeView",
  components: {MessageWindow}
});
</script>

<style scoped>

</style>
