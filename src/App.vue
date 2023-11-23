<template>
  <div id="app">
    <h1>AI Chatbot</h1>
    <div>
      <input type="password" v-model="apiKey" placeholder="Enter OpenAI API Key" />
      <button @click="saveApiKey">Save API Key</button>
    </div>
    <div class="chat-window">
      <ul>
        <li v-for="message in messages" :key="message.id" :class="{'user-message': message.type === 'user', 'ai-message': message.type === 'ai'}">
          {{ message.text }}
        </li>
      </ul>
    </div>
    <input type="text" v-model="userInput" @keyup.enter="sendMessage" placeholder="Type your message here"/>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      apiKey: '',
      userInput: '',
      messages: [],
      nextId: 0
    };
  },
  methods: {
    addMessage(type, text) {
      this.messages.push({ id: this.nextId++, type, text });
    },
    async fetchAIResponse(prompt) {
      if (!this.apiKey.trim()) {
        alert('API key is required');
        return;
      }

      try {
        const response = await axios.post('https://api.openai.com/v1/engines/davinci-codex/completions', {
          prompt: prompt,
          max_tokens: 150
        }, {
          headers: {
            'Authorization': `Bearer ${this.apiKey}`,
            'Content-Type': 'application/json'
          }
        });

        this.addMessage('ai', response.data.choices[0].text.trim());
      } catch (error) {
        console.error('Error fetching AI response:', error);
        alert('Failed to fetch AI response. Check the console for more details.');
      }
    },
    sendMessage() {
      if (!this.userInput.trim()) return;

      this.addMessage('user', this.userInput);

      this.fetchAIResponse(this.userInput);

      this.userInput = '';
    },
    saveApiKey() {
      if (!this.apiKey.trim()) {
        alert('Please enter a valid API key');
        return;
      }
      alert('API Key saved! You can now interact with the AI.');
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
  background-color: #f9f9f9;
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
input[type="text"], input[type="password"] {
  width: 70%;
  padding: 10px;
  margin-bottom: 10px;
}
button {
  padding: 10px 20px;
  cursor: pointer;
}
</style>

