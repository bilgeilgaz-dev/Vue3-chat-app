<script setup>
import { onMounted, ref } from 'vue';
import { v4 as uuidv4 } from 'uuid';

const connection = ref(null);
const message = ref('');
const messages = ref([]);
const userId = ref(uuidv4());

onMounted(() => {
  connection.value = new WebSocket('wss://echo.websocket.org');

  connection.value.onmessage = (event) => {
    messages.value.push({ userId: userId, text: event.data });
  };

  connection.value.onopen = () => {
    console.log('Connected to the WebSocket server');
  };

  connection.value.onclose = () => {
    console.log('Disconnected from the WebSocket server');
  };
});

function sendMessage() {
  connection.value.send(message.value);
  message.value = '';
}
</script>

<template>
  <div>
    <h1>Real-Time Chat</h1>
    <input type="text" v-model="message" @keyup.enter="sendMessage">
    <ul>
      <li v-for="message in messages" :key="message.id">
        {{ message.text }}
      </li>
    </ul>
  </div>
</template>

<style scoped>

</style>
