<template>
  <div class="chat-widget">
    <div class="widget-header">
      Chat Bot
    </div>
    <div class="messages" ref="messagesContainer">
      <Message v-for="(message, index) in messages" :key="index" :message="message" @button-click="handleButtonClicked"/>
    </div>
    <div class="input-area">
      <input class="input" v-model="inputMessage" @keyup.enter="sendMessage" placeholder="Введите сообщение..." />
      <button class="btn-send" @click="sendMessage"  :disabled="isBotResponding">
        <svg height="30px" width="30px" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 512 512" xml:space="preserve">
          <polygon style="fill:#027372;" points="97.478,235.728 147.096,478.242 512,33.758 "/>
          <polygon style="fill:#02ACAB;" points="251.837,373.231 147.096,478.242 164.932,325.531 231.773,327.36 "/>
          <g>
            <polygon style="fill:#81E3E2;" points="512,33.758 109.455,294.271 0,232.606   "/>
            <polygon style="fill:#81E3E2;" points="512,33.758 511.471,35.232 300.246,399.799 164.932,325.531  "/>
          </g>
          <polygon style="fill:#42C8C6;" points="300.246,399.799 511.471,35.232 367.526,436.73 "/>
        </svg>
      </button>
    </div>
  </div>
</template>
<script>
import Message from './ChatMessage.vue';
const delayValue = 1000;
export default {
  components: {
    Message,
  },
  data() {
    return {
      messages: [],
      inputMessage: '',
      isBotResponding: false,
    };
  },
  mounted() {
    this.startBotGreeting();
  },
  methods: {
    async startBotGreeting() {
      this.messages.push({
        text: '...',
        isUser: false,
      });
      await this.delay(delayValue);
      this.messages.pop();
      this.messages.push({
        text: 'Привет! Как я могу вам помочь?',
        isUser: false,
      });
      //кнопки
      this.messages.push({
        buttonTexts: ['Заказать пиццу', 'Установить будильник', 'Вывести погоду'],
        isUser: false,
        isButton: true,
      });
      this.$nextTick(() => {
        this.scrollMessagesToBottom();
      });
    },
    async handleButtonClicked(buttonText) {
      this.isBotResponding = true;
      this.messages.pop();
      // Отображение выбранной кнопки как сообщения
      this.messages.push({
        text: '...',
        isUser: true,
      });
      this.$nextTick(() => {
        this.scrollMessagesToBottom();
      });
      await this.delay(delayValue);
      this.messages.pop();
      this.messages.push({
        text: `${buttonText}`,
        isUser: true,
      });

      this.$nextTick(() => {
        this.scrollMessagesToBottom();
      });

      await this.delay(delayValue);

      this.messages.push({
        text: '...',
        isUser: false,
      });
      this.$nextTick(() => {
        this.scrollMessagesToBottom();
      });
      await this.delay(delayValue);
      this.messages.pop();

      // Ответ бота на выбор кнопки
      if (buttonText === 'Заказать пиццу') {
        this.messages.push({
          text: 'Хорошо, я закажу вам пепперони. Что еще могу сделать?',
          isUser: false,
        });
      } else if (buttonText === 'Установить будильник') {
        this.messages.push({
          text: 'Хорошо, я поставлю будильник на 7:00. Что еще могу сделать?',
          isUser: false,
        });
      } else if (buttonText === 'Вывести погоду') {
        this.messages.push({
          text: 'По моим данным, сейчас солнечно, 25°C. Что еще могу сделать?',
          isUser: false,
        });
      }

      this.isBotResponding = false;

      // Следующие кнопки
      this.messages.push({
        buttonTexts: ['Заказать пиццу', 'Установить будильник', 'Вывести погоду'],
        isUser: false,
        isButton: true,
      });

      this.$nextTick(() => {
        this.scrollMessagesToBottom();
      });
    },
    async sendMessage() {
      let messageToSend = 0;

      if (this.inputMessage.trim() === '') return;
      this.messages.pop();
      messageToSend = this.inputMessage.trim();
      this.isBotResponding = true;
      this.inputMessage = '';
      this.messages.push({
        text: '...',
        isUser: true,
      });
      this.$nextTick(() => {
        this.scrollMessagesToBottom();
      });
      await this.delay(delayValue);
      this.messages.pop();
      this.messages.push({
        text: messageToSend,
        isUser: true,
      });
      this.$nextTick(() => {
        this.scrollMessagesToBottom();
      });
      await this.delay(delayValue);
      this.messages.push({
        text: '...',
        isUser: false,
      });
      this.$nextTick(() => {
        this.scrollMessagesToBottom();
      });
      await this.delay(delayValue);
      this.messages.pop();
      this.messages.push({
        text: 'Эти глупые программисты еще не научили меня отвечать прямо на ваш текстовый запрос. Выберите пожалуйста один из предложенных вариантов:',
        isUser: false,
      });
      this.messages.push({
        buttonTexts: ['Заказать пиццу', 'Установить будильник', 'Вывести погоду'],
        isUser: false,
        isButton: true,
      });
      this.isBotResponding = false;
      this.$nextTick(() => {
        this.scrollMessagesToBottom();
      });
    },
    async delay(ms) {
      return new Promise(resolve => setTimeout(resolve, ms));
    },
    scrollMessagesToBottom() {
      const container = this.$refs.messagesContainer;
      container.scrollTop = container.scrollHeight;
    },
  },
};
</script>

<style scoped>
.widget-header{
  height: 10%;
  background-color: #306BAC;
  color: #f8f8f8;
  padding: 16px;
  box-sizing: border-box;
  font-weight: 700;
}
.chat-widget{
  width: 500px;
  height: 500px;
  border-radius: 16px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  background-color: #CAE9FF;

  -webkit-box-shadow: 0px 9px 19px 0px rgba(67,67,82,1);
    -moz-box-shadow: 0px 9px 19px 0px rgba(67,67,82,1);
    box-shadow: 0px 9px 19px 0px rgba(67,67,82,1);
}
.messages{
  display: flex;
  flex-direction: column;
  height: 90%;
  overflow-y: auto;
}
.input-area{
  height: 10%;
  display: flex;
  justify-content: space-between;
}
.input{
  border: none;
  width: 100%;
  background-color: #f8f8f8;
  padding: 16px;
}:focus{
  outline: none;
}
.btn-send{
  border: none;
  background-color: #f8f8f8;
  cursor: pointer;
  padding: 0px 16px 0px 16px;
}
.btn-send:hover svg{
  opacity: 0.5;
}
</style>
