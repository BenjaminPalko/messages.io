<script lang="ts">
import io from 'socket.io-client';
import { defineComponent } from "vue";

interface ChatMessage {
  username: string;
  message: string;
}

export default defineComponent({
  name: "MessageWindow",
  data() {
    return {
      message: '',
      messages: [] as ChatMessage[],
      socket: io(import.meta.env.VITE_SOCKET_SERVER)
    }
  },
  methods: {
    sendMessage() {
      if (this.socket && this.message.length > 0) {
        this.socket.emit("publish message", this.message);
        this.message = '';
      }
    }
  },
  mounted() {
    console.log(import.meta.env.VITE_SOCKET_SERVER);
    this.socket.on('connect', () => {
      console.log(`Socket open`);
      this.socket.on("subscribe message", args => this.messages.push(args))
    });
  }
});
</script>

<template>
  <div class="container mx-auto message-window bg-gray-100 rounded shadow px-10">
    <div class="grid-cols-1 place-content-center h-full">
      <p v-for="(chatMessage, index) of messages" :key="index">
        {{ chatMessage.username }}: {{ chatMessage.message }}
      </p>
    </div>
    <div class="grid grid-cols-4 gap-2 place-content-center h-20">
      <input v-model="message" class="py-1 shadow border col-span-3 p-2" placeholder="Send a message"/>
      <button type="button" class="bg-blue-500 rounded text-white hover:bg-blue-700 font-bold py-2 px-4 shadow"
              @click="sendMessage">Send Message
      </button>
    </div>
  </div>
</template>

<style scoped>
.message-window {
  max-width: 720px;
}
</style>
