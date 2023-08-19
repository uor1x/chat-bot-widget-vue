<template>
  <div class="message-container">
    <div v-if="!message.isButton" class="message-text-container" :class="{ userContainer: message.isUser, botContainer: !message.isUser }">
      <img class="message-avatar" :class="{userAvatarBG: message.isUser, botAvatarBG: !message.isUser}" :src="avatarSrc" alt="">
      <div class="chat-message" :class="{ user: message.isUser, bot: !message.isUser }">
        <span v-if="message.text === '...'" class="typingAnim">{{ message.text }}</span>
        <span v-else>{{ message.text }}</span>
      </div>
    </div>
    <div v-if="message.isButton" class="button-container">
      <button v-for="(btnText, index) in message.buttonTexts" :key="index" class="message-button" @click="handleButtonClick(btnText)">{{ btnText }}</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ['message'],
  computed: {
    avatarSrc() {
      return this.message.isUser ? require('../assets/user-avatar.png') : require('../assets/bot-avatar.png');
    },
  },
  methods:{
    handleButtonClick(buttonText) {
      console.log(buttonText)
      this.$emit('button-click', buttonText);
    },
  },
};
</script>

<style scoped>
/* Add your styling here */

.message-container{
}

.message-text-container{
  display: flex;
  gap: 10px;
  flex-direction: row;
  align-items: flex-end;
  box-sizing: border-box;
  margin: 10px;
}
.userContainer{
  flex-direction: row-reverse;

}

.botContainer{

}
.chat-message {
  max-width: 80%;
  padding: 8px;

  box-shadow: 0px 1px 3px 0px rgba(0,0,0,0.51);
  -webkit-box-shadow: 0px 1px 3px 0px rgba(0,0,0,0.51);
  -moz-box-shadow: 0px 1px 3px 0px rgba(0,0,0,0.51);
}

.user {
  background-color: #3498db;
  color: #f8f8f8;

  align-self: flex-end;

  border-radius: 16px 16px 0px 16px;
}

.bot {
  background-color: #306BAC;
  color: #f8f8f8;
  border-radius: 16px 16px 16px 0px;
}

.message-avatar{
  width: 32px;
  height: 32px;
  background-color: #f8f8f8;
  padding: 2px;

  box-shadow: 0px 1px 4px 0px rgba(0,0,0,0.51);
  -webkit-box-shadow: 0px 1px 4px 0px rgba(0,0,0,0.51);
  -moz-box-shadow: 0px 1px 4px 0px rgba(0,0,0,0.51);
  
}

.userAvatarBG{
  border-radius: 16px  16px 16px 0px;
}

.botAvatarBG{
  border-radius: 16px 16px 0px 16px;
}
.button-container{
  max-width: 80%;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 16px;
  margin-left: 55px;
  margin-bottom: 10px;
  box-sizing: border-box;
}

.message-button{
  background-color: #306BAC;
  outline: none;
  border: none;
  border-radius: 16px;
  padding: 8px 16px;
  color: #f8f8f8;
  cursor: pointer;
}
.message-button:hover{
  background-color: #507dad;
}

.typingAnim {
  animation: dots-anim 2s ease-in-out 0s infinite normal forwards;
}

@keyframes dots-anim {
	0%,
	50%,
	100% {
		opacity: 1;
	}

	25%,
	75% {
		opacity: 0;
	}
}
</style>
