<template>
  <div id="app" class="borderapp">
    <div class="container">
      <div class="row">
        <div class="col-12">
          <div class="announcement">
            欢迎进入chatgpt聊天系统
          </div>
          <div class="chat-container">
            <ChatBubble
              v-for="(message, index) in messages"
              :key="index"
              :type="message.type"
              :message="message.text"
            />
          </div><br />
          <MessageInput @send-message="addMessage" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ChatBubble from "./components/ChatBubble.vue";
import MessageInput from "./components/MessageInput.vue";
import axios from 'axios';
import qs from 'qs';
export default {
  components: {
    ChatBubble,
    MessageInput
  },
  data() {
    return {
      messages: [
        { type: "sender", text: "你好" },
        { type: "receiver", text: "我很好" },
      ],
    };
  },
  methods: {
    addMessage(text, type = 'sender') {
      console.log("2222222222222222");
      this.messages.push({ type, text });
  
      if (type === 'sender') {
        this.fetchMessages();
      }
    },
    async fetchMessages() {
      console.log("111111111111111111111");
      setTimeout(async () => {
        try {
          // 在发送请求之前调用 setSession
          const sessionSet = await this.setSession(this.messages);
    
          if (!sessionSet) {
            this.fetchMessages();
            return;
          }
    
          const response = await axios.get(`http://43.154.193.86/stream.php?message=${encodeURIComponent(this.messages[this.messages.length - 1].text)}`);
          console.log("请求返回：", response);
    
          if (response.status === 200) {
            this.messages.push({ type: "receiver", text: response.data.text });
          }
          this.fetchMessages();
        } catch (error) {
          console.error("获取消息失败，错误信息：", error);
          this.fetchMessages();
        }
      }, 1000);
    },
    async setSession(messages) {
      try {
        const response = await axios.post('http://43.154.193.86/setsession.php', {
          context: JSON.stringify(messages),
        });

        if (response.data.success) {
          return true;
        } else {
          console.error('设置 session 失败');
          return false;
        }
      } catch (error) {
        console.error('设置 session 失败，错误信息：', error);
        return false;
      }
    },
  },

  created() {
    this.fetchMessages();
  },
};
</script>


<style>
#app {
  font-family: Arial, sans-serif;
  padding: 10px;
  height: 100%;
}

.chat-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  max-width: 100%;
  margin: 0 auto;
  padding: 14px;
  background-color: #f5f5f5;
  border-radius: 8px;
  height: calc(100vh - 120px); /* 减去底部输入框的高度和上下 padding */
  overflow-y: scroll;
}
.borderapp{
  border-radius: 20px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
}

.announcement {
  background-color: #f5f5f5;
  color: red;
  padding: 10px;
  border-radius: 10px;
  text-align: center;
  margin-bottom: 10px;
}
</style>
