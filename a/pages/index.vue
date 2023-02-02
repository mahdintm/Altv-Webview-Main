<template>
  <div>
    <ChatSystem />
  </div>
</template>

<script>
import ChatSystem from '~/components/chat/baseChat.vue'
export default {
  comments: [ChatSystem],
  name: 'IndexPage',
  data() {
    return {
      show: {
        chat: true,
      },
    }
  },
}
</script>
