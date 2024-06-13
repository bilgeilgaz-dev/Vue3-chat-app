<script setup>
import { onMounted, ref } from 'vue';
import { v4 as uuidv4 } from "uuid";

const connection = ref(null);
const message = ref('');
const messages = ref([]);
const userId = uuidv4();

onMounted(() => {
  connection.value = new WebSocket('wss://echo.websocket.org');
  connection.value.onopen = () => {
    console.log('Connected to the WebSocket server');
  };

  connection.value.onmessage = (event) => {
    console.log('Received:', event.data);
    messages.value.push({ userId, text: event.data });
  };
  

  connection.value.onclose = () => {
    console.log('Disconnected from the WebSocket server');
  };
});

function sendMessage() {
  if(!message.value) return;
  connection.value.send(message.value);
  message.value = '';
}
</script>

<template>
  <div>
    <h1>Real-Time Chat</h1>
    <form @submit.prevent="sendMessage">
    <input type="text" v-model="message">
    <button>+</button>
  </form>
  <ul>
      <li v-for="message in messages" :key="message.id">
        {{ message }}
      </li>
    </ul>
  </div>
  
</template>

<style scoped>

</style>
