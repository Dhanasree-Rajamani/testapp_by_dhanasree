<template>
  <div id="app">
    <h1>AI Chatbot</h1>
    <div class="chat-window">
      <ul>
        <li v-for="message in messages" :key="message.id" :class="{'user-message': message.type === 'user', 'ai-message': message.type === 'ai'}">
          {{ message.text }}
        </li>
      </ul>
    </div>
    <input type="text" v-model="userInput" @keyup.enter="sendMessage"/>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      userInput: '',
      messages: [],
      nextId: 0
    }
  },
  methods: {
    sendMessage() {
      if (!this.userInput.trim()) return;

      // Add user message
      this.addMessage('user', this.userInput);

      // Send the user input to the AI backend and get the response
      // For now, we'll just mock this
      setTimeout(() => {
        this.addMessage('ai', `AI Response to: ${this.userInput}`);
      }, 1000);

      // Clear the user input
      this.userInput = '';
    },
    addMessage(type, text) {
      this.messages.push({ id: this.nextId++, type: type, text: text });
    }
  }
}
</script>

<style>
#app {
  max-width: 600px;
  margin: 40px auto;
  text-align: center;
}
.chat-window {
  border: 1px solid #ddd;
  padding: 20px;
  height: 300px;
  overflow-y: auto;
  margin-bottom: 20px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin-bottom: 10px;
  word-break: break-all;
}
.user-message {
  color: blue;
  text-align: right;
}
.ai-message {
  color: green;
  text-align: left;
}
input[type="text"] {
  width: 100%;
  padding: 10px;
}
</style>

