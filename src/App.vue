<template>
  <div id="app">
    <ChatWidget />
  </div>
</template>

<script>
import ChatWidget from './components/ChatWidget.vue';

export default {
  name: 'App',
  components: {
    ChatWidget,
  },
};
</script>

<style>
  #app{
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;

    font-family: 'Montserrat', sans-serif;
  }
  body{
    background-image: url('./assets/pattern-background-1-scaled.jpg');
    background-repeat: no-repeat;
    background-size: cover;
    margin: 0;
  }
</style>
