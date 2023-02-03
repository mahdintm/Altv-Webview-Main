<template>
  <b-container style="background-color: #111111; height: 100vh" fluid>
    <b-row align-h="between" class="text-center h-50">
      <b-col
        cols="7"
        sm="6"
        md="6"
        lg="5"
        xl="4"
        class="p-2"
        style="height: 73%"
        ><ChatSystem
      /></b-col>
      <b-col cols="3" sm="3" md="3" lg="2" xl="2" class="hud h-50"
        >HUD <br />
        HUD
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import ChatSystem from '~/components/chat/baseChat.vue'
export default {
  components: { ChatSystem },
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
<style>
.hud {
  /* background-color: aqua; */
}
</style>
