<!-- components/ChatBubble.vue -->
<template>
  <div class="chat-row" :class="type">
	<font-awesome-icon icon="user" class="avatar" />
    <div class="chat-bubble" :class="type">
      <p>{{ message }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    type: {
      type: String,
      required: true
    },
    message: {
      type: String,
      required: true
    }
  },
  computed: {
    avatarUrl() {
      return new URL('../assets/logo.png', import.meta.url).href;
    }
  },
}
</script>

<style scoped>
.chat-row {
  display: flex;
  align-items: flex-end;
  margin-bottom: 16px;
  width: 100%;
}

.chat-row.sender {
  flex-direction: row-reverse;
}

.avatar {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  object-fit: cover;
  margin: 0 8px;
  color: #ff5500;
}

.chat-bubble {
  position: relative;
  max-width: 70%;
  padding: 3px 5px 0px 8px;
  background-color: #ffcc80;
  border-radius: 12px;
}
.chat-bubble P{
  margin: 8px 1px 8px;
}
.chat-bubble.sender {
  background-color: #0b93f6;
  color: #fff;
}

.chat-bubble.receiver:before {
  content: "";
  position: absolute;
  top: 0;
  left: -10px;
  width: 0;
  height: 0;
/*  border-top: 10px solid transparent;
  border-right: 10px solid #ffcc80;
  border-bottom: 10px solid transparent; */
}

.chat-bubble.sender:before {
  content: "";
  position: absolute;
  top: 0;
  right: -10px;
  width: 0;
  height: 0;
 /* border-top: 10px solid transparent;
  border-left: 10px solid #0b93f6;
  border-bottom: 10px solid transparent; */
}


</style>
