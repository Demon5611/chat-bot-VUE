<template>
  <div class="container">
    <div class="chat-widget">
      <div class="chat-messages">
        <div v-for="(message, index) in messages" :key="index" class="message">
          <div :class="message.from === 'user' ? 'user-message' : 'bot-message'">
            {{ message.text }}
          </div>
        </div>
      </div>
      <div class="user-input">
        <input
          v-model="userMessage"
          @keyup.enter="sendMessage"
          type="text"
          class="form-control"
          placeholder="Type your message..."
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      messages: [],
      userMessage: '',
      options: ['Заказать пиццу', 'Установить будильник', 'Вывести погоду']
    }
  },
  methods: {
    sendMessage() {
      const userText = this.userMessage.trim()
      if (userText !== '') {
        this.messages.push({ text: userText, from: 'user' })
        this.userMessage = ''

        // Handle user input
        this.handleUserInput(userText)
      }
    },
    handleUserInput(userText) {
      if (userText.toLowerCase() === 'заказать пиццу') {
        this.messages.push({ text: 'Хорошо, я закажу пиццу. Что еще могу сделать?', from: 'bot' })
      } else if (userText.toLowerCase() === 'установить будильник') {
        this.messages.push({ text: 'На какое время установить будильник?', from: 'bot' })
      } else if (userText.toLowerCase() === 'вывести погоду') {
        this.messages.push({
          text: 'К сожалению, я не могу предоставить погоду в данный момент.',
          from: 'bot'
        })
      } else {
        this.messages.push({ text: 'Извините, я не понял ваш запрос.', from: 'bot' })
      }
    }
  }
}
</script>

<style scoped>
.chat-widget {
  border: 1px solid #ccc;
  border-radius: 5px;
  overflow: hidden;
  max-width: 400px;
  margin: 20px auto;
}

.chat-messages {
  max-height: 600px;
  overflow-y: auto;
  padding: 10px;
  margin-bottom: 3ch;
}

.message {
  margin-bottom: 6ch;
}

.user-message {
  background-color: #cce5ff;
  padding: 5px 10px;
  border-radius: 5px;
  float: right;
  margin-bottom: 1ch; /* Уменьшил отступ между сообщениями пользователя */
  margin-top: 1ch;
}

.bot-message {
  background-color: #e0e0e0;
  padding: 10px 10px;
  border-radius: 5px;
  float: left;
  margin-bottom: 1ch; /* Уменьшил отступ между сообщениями бота */
  margin-top: 1ch;
}

.user-input {
  padding: 10px;
  background-color: #f5f5f5;
  border-top: 1px solid #ccc;
}

input {
  width: 100%;
}
</style>
