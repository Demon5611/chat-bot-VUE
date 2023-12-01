<template>
  <div class="container">
    <div class="chat-widget">
      <div class="chat-messages">
        <!-- приветственное сообщение -->
        <div class="message bot-message">Привет! Что я могу для Вас сделать?</div>
        <!--  кнопки с вариантами продолжения общения -->
        <div class="options">
          <button @click="handleOptionClick('Заказать пиццу')">Заказать пиццу</button>
          <button @click="handleOptionClick('Установить будильник')">Установить будильник</button>
          <button @click="handleOptionClick('Вывести погоду')">Вывести погоду</button>
        </div>
        <!-- Вывод сообщений чата -->
        <div v-for="(message, index) in messages" :key="index" class="message">
          <div class="avatar-container">
            <img
              v-if="message.from === 'user'"
              style="width: 30px; height: 30px; margin-left: 40ch"
              src="data:image/svg+xml,%3csvg viewBox='-208.5 21 100 100' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3e%3ccircle cx='-158.5' cy='71' fill='%23F5EEE5' r='50'/%3e%3cdefs%3e%3ccircle cx='-158.5' cy='71' id='a' r='50'/%3e%3c/defs%3e%3cclipPath id='b'%3e%3cuse overflow='visible' xlink:href='%23a'/%3e%3c/clipPath%3e%3cpath clip-path='url(%23b)' d='M-108.5 121v-14s-21.2-4.9-28-6.7c-2.5-.7-7-3.3-7-12V82h-30v6.3c0 8.7-4.5 11.3-7 12-6.8 1.9-28.1 7.3-28.1 6.7v14h100.1z' fill='%23E6C19C'/%3e%3cg clip-path='url(%23b)'%3e%3cdefs%3e%3cpath d='M-108.5 121v-14s-21.2-4.9-28-6.7c-2.5-.7-7-3.3-7-12V82h-30v6.3c0 8.7-4.5 11.3-7 12-6.8 1.9-28.1 7.3-28.1 6.7v14h100.1z' id='c'/%3e%3c/defs%3e%3cclipPath id='d'%3e%3cuse overflow='visible' xlink:href='%23c'/%3e%3c/clipPath%3e%3cpath clip-path='url(%23d)' d='M-158.5 100.1c12.7 0 23-18.6 23-34.4 0-16.2-10.3-24.7-23-24.7s-23 8.5-23 24.7c0 15.8 10.3 34.4 23 34.4z' fill='%23D4B08C'/%3e%3c/g%3e%3cpath d='M-158.5 96c12.7 0 23-16.3 23-31 0-15.1-10.3-23-23-23s-23 7.9-23 23c0 14.7 10.3 31 23 31z' fill='%23F2CEA5'/%3e%3c/svg%3e"
              alt="user-avatar"
            />
            <img
              v-else
              style="width: 30px; height: 30px"
              src="data:image/svg+xml,%3csvg version='1' xmlns='http://www.w3.org/2000/svg' viewBox='0 0 512 512'%3e%3cpath d='M303 70a47 47 0 1 0-70 40v84h46v-84c14-8 24-23 24-40z' fill='%2393c7ef'/%3e%3cpath d='M256 23v171h23v-84a47 47 0 0 0-23-87z' fill='%235a8bb0'/%3e%3cpath fill='%2393c7ef' d='M0 240h248v124H0z'/%3e%3cpath fill='%235a8bb0' d='M264 240h248v124H264z'/%3e%3cpath fill='%2393c7ef' d='M186 365h140v124H186z'/%3e%3cpath fill='%235a8bb0' d='M256 365h70v124h-70z'/%3e%3cpath fill='%23cce9f9' d='M47 163h419v279H47z'/%3e%3cpath fill='%2393c7ef' d='M256 163h209v279H256z'/%3e%3cpath d='M194 272a31 31 0 0 1-62 0c0-18 14-32 31-32s31 14 31 32z' fill='%233c5d76'/%3e%3cpath d='M380 272a31 31 0 0 1-62 0c0-18 14-32 31-32s31 14 31 32z' fill='%231e2e3b'/%3e%3cpath d='M186 349a70 70 0 1 0 140 0H186z' fill='%233c5d76'/%3e%3cpath d='M256 349v70c39 0 70-31 70-70h-70z' fill='%231e2e3b'/%3e%3c/svg%3e"
              alt="bot-avatar"
            />
          </div>
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
    // Обработка нажатия на кнопку с вариантом выбора ответа - добавляем в массив и передаем далее в handleUserInput
    handleOptionClick(option) {
      this.messages.push({ text: option, from: 'user' })
      // то, что пользователь выбрал из опций ответов - улетает в чат
      this.handleUserInput(option)
    },
    sendMessage() {
      const userText = this.userMessage.trim()
      if (userText !== '') {
        this.messages.push({ text: userText, from: 'user' })
        this.userMessage = ''
        // положили в хендлер текст от пользователя
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
button {
  background-color: rgb(206, 241, 238);
  border-radius: 10px;
  border: 1px solid #b0a9a95c;
  width: 225px;
}
button:hover {
  background-color: rgb(172, 232, 227);
  transform: scale(1.01);
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

.options {
  margin-top: 1ch;
  margin-bottom: 3ch;
}

button {
  margin-right: 1ch;
}
</style>
