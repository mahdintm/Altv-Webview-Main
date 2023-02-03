<template>
  <div id="Main_Chat">
    <div fulid id="Chat_Display">
      <div
        v-for="item of chats"
        :key="item.chat"
        :chat="item.chat"
        :time="item.time"
      >
        <span>[{{ item.time }}] </span>
        <span>{{ colorify(item.chat) }}</span>
      </div>
    </div>
    <b-form-input
      v-if="show_Input"
      style="height: 13%"
      autocomplete="off"
      spellcheck="false"
    ></b-form-input>
  </div>
</template>
<script>
export default {
  name: 'ChatSystem',
  data() {
    return {
      show_Input: true,
      Shoe_Time: false,
      chats: [],
    }
  },
  unmounted() {
    if ('alt' in window) {
    }
  },
  mounted() {
    if ('alt' in window) {
    }
    // this.gotoZeroDownScroll()
    for (let i = 1; i <= 60; i++) {
      this.addchat(1675445829983, `in {FFFFFF} test ${i + 1}`)
    }
  },
  methods: {
    colorify(text) {
      let matches = []
      let m = null
      let curPos = 0
      do {
        m = /\{[A-Fa-f0-9]{3}\}|\{[A-Fa-f0-9]{6}\}/g.exec(text.substr(curPos))
        if (!m) break
        matches.push({
          found: m[0],
          index: m['index'] + curPos,
        })
        curPos = curPos + m['index'] + m[0].length
      } while (m != null)
      if (matches.length > 0) {
        text += '</span>'
        for (let i = matches.length - 1; i >= 0; --i) {
          let color = matches[i].found.substring(1, matches[i].found.length - 1)
          let insertHtml =
            (i != 0 ? '</span>' : '') + '<span style="color: #' + color + '">'
          text =
            text.slice(0, matches[i].index) +
            insertHtml +
            text.slice(matches[i].index + matches[i].found.length, text.length)
        }
      }
      return text.replaceAll('\n', '<br>')
    },
    addchat(TIME_SERVER, text) {
      let TimeStamp = new Date(TIME_SERVER)
      let Time = `${this.addziro(TimeStamp.getHours())}:${this.addziro(
        TimeStamp.getMinutes()
      )}:${this.addziro(TimeStamp.getSeconds())}`
      this.pushMessage({ time: Time, chat: text })
      // await this.gotoZeroDownScroll()
    },
    gotoZeroDownScroll() {
      document
        .getElementById('Chat_Display')
        .scrollTo(0, document.getElementById('Chat_Display').scrollHeight)
    },
    pushMessage(obj) {
      if (this.chats.length > 50) this.chats.shift()
      this.chats.push(obj)
    },
    addziro(adad) {
      if (adad < 10) {
        return `0${adad}`
      } else {
        return adad
      }
    },
  },
}
</script>
<style>
#Main_Chat {
  height: 100% !important;
}
#Chat_Display {
  background-color: red;
  height: 85%;
  text-align: left;
  padding-left: 2%;
  overflow-wrap: break-word;
  overflow-y: scroll;
}
#Chat_Display p {
  margin: 0 !important;
}
</style>
