<template>
  <div class="container">
    <div class="conversation shadow">
      <div
        v-for="message in state.messages"
        :key="message"
        class="message"
        :class="{ 'is-user': message.isFromUser }"
      >
        <div class="avatar">
          <img :src="message.avatar" alt="avatar" />
        </div>
        <div class="content">
          <div class="text">
            {{ message.text }}
          </div>
          <div class="time">
            {{ message.time }}
          </div>
        </div>
      </div>
    </div>
    <div class="input-container">
      <!-- @keydown.enter="sendMessage" -->
      <input v-model="state.userInput" type="text" placeholder="Type here..." />
      <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<script setup>
import { reactive } from "vue";
import axios from "axios";

const state = reactive({
  userInput: "",
  messages: [
    {
      isFromUser: true,
      avatar: "https://via.placeholder.com/50x50?text=User",
      text: "Hello, how can I help you?",
      time: "1 minute ago",
    },
    {
      isFromUser: false,
      avatar: "https://via.placeholder.com/50x50?text=GPT",
      text: "Hello! I am GPT. Please ask me any question you have.",
      time: "1 minute ago",
    },
  ],
});

const sendMessage = async () => {
  try {
    if (state.userInput.trim() === "") {
      return;
    }
    const res = await axios.get(
      `https://107.191.49.166:9615/gpt/answers?message=${state.userInput}`
    );
    console.log(res);
    state.userInput = "";
  } catch (error) {
    console.log(error);
  }
};
</script>

<style scoped>
.shadow {
  -webkit-box-shadow: #666 0px 0px 10px;
  -moz-box-shadow: #666 0px 0px 10px;
  box-shadow: #666 0px 0px 10px;
}
.container {
  min-width: 800px;
  width: 60%;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100vh;
}

.conversation {
  flex: 1;
  overflow-y: auto;
  padding: 10px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  border-radius: 10px;
}

.conversation::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}

.conversation::-webkit-scrollbar-thumb {
  background-color: rgba(0, 0, 0, 0.3);
  border-radius: 10px;
}

.conversation::-webkit-scrollbar-track {
  background-color: rgba(0, 0, 0, 0.1);
  border-radius: 10px;
}

.message {
  display: flex;
  margin-bottom: 10px;
  animation: fadeIn 0.5s ease-out;
  padding: 0.75rem;
  background-color: rgb(229, 231, 235);
  border-radius: 10px;
}

.is-user {
  justify-content: flex-end;
}

.is-user .avatar {
  margin-left: 10px;
  margin-right: 0;
}

.is-user .content {
  align-items: flex-end;
}

.avatar {
  margin-right: 10px;
  margin-left: 0;
}

.avatar img {
  width: 50px;
  height: 50px;
  border-radius: 50%;
}

.content {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  max-width: 80%;
}

.text {
  padding: 10px;
  border-radius: 10px;
  background-color: #f5f5f5;
  color: #333;
  font-size: 16px;
  line-height: 1.5;
}

.time {
  font-size: 12px;
  color: #999;
  margin-top: 5px;
}

.input-container {
  display: flex;
  align-items: center;
  background-color: #f5f5f5;
  padding: 10px;
  border-radius: 10px;
  margin: 10px 0px 20px 0px;
  animation: slideInUp 0.5s ease-out;
}

input[type="text"] {
  flex: 1;
  padding: 10px;
  font-size: 16px;
  background-color: #f5f5f5;
  outline: none;
  border: 1px solid #e5e7eb;
  border-radius: 10px;
}

input[type="text"]:disabled {
  opacity: 0.5;
}

button {
  padding: 10px 20px;
  background-color: #4caf50;
  border: none;
  color: white;
  font-size: 16px;
  border-radius: 5px;
  margin-left: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease-out;
}

button:hover {
  background-color: #3e8e41;
}

@keyframes slideInUp {
  0% {
    transform: translateY(100%);
  }

  100% {
    transform: translateY(0);
  }
}

@keyframes fadeIn {
  0% {
    opacity: 0;
  }

  100% {
    opacity: 1;
  }
}
</style>
