<!-- components/MessageInput.vue -->
<template>
  <div class="input-group">
    <input
      type="text"
      class="form-control"
      placeholder="请输入您的对话"
      v-model="messageInput"
      @keyup.enter="sendMessage"
    />
    <button class="btn btn-primary" type="button" @click="sendMessage">
      <font-awesome-icon icon="paper-plane" class="send-icon" @click="sendMessage" />
    </button>
  </div>
</template>

<script>
	import axios from 'axios';
	
	// const api = axios.create({
	//   baseURL: 'http://chatgpt2.chat.colinyuyu.com/api', // 请替换为你的后台 API 地址
	//   timeout: 1000,
	//   headers: { 'Content-Type': 'application/json'},
	// });

export default {
  data() {
    return {
      messageInput: ''
    }
  },
  methods: {
    sendMessage() {
      if (!this.messageInput.trim()) return;
      this.$emit("send-message", this.messageInput);
      this.messageInput = "";
    },
  },

}

</script>

<style>
.send-icon {
  cursor: pointer;
  color: #f3f3f3;
  font-size: 24px;
}
</style>
