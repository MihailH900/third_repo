<template>
    <div id="app">
        <Container>
            <ChatWindow @send-message = "sendMessage">
                <ChatMessage v-for="message in messages" :key = "message.id" :username="message.author" :datetime="message.datetime">
                </ChatMessage>
            </ChatWindow>
        </Container>
    </div>
</template>

<script>
import axios from 'axios'
import ChatMessage from './components/ChatMessage.vue'
import ChatWindow from './components/ChatWindow.vue'
import Container from './components/Container.vue'

export default {
  name: 'app',
  data()
  {
      return {messages: []}
  },
  methods:
  {
      sendMessage(obj){
          axios.post('http://188.225.47.187/api/chat/sendmessage.php',{
              author: obj.nickname,
              text: obj.message
          }).then(()=>{
              this.getMessages()
          })
      },

      getMessages(){
          axios.get('http://188.225.47.187/api/chat/getmessages.php')
          .then(response=>{
              this.messages = response.data.sort((a, b) => a.id - b.id)
          })
      },
      mounted(){
          setInterval(()=>{
              this.getMessages()
          }, 3000)
          this.getMessages()
      }
  },
  components: {
      ChatMessage, ChatWindow, Container
  }
}
</script>

<style>
    body
    {
        margin: 0;
        background-color: #f9f9fa;
    }
</style>
