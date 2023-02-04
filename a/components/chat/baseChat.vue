<template>
  <div id="Main_Chat">
    <div
      fulid
      id="Chat_Display"
      :class="show_Input ? 'activeScroll canSelectText' : ''"
    >
      <div v-for="item of chats" :key="item.chat">
        <span>[{{ item.time }}] </span>
        <span v-html="colorify(item.chat)"></span>
      </div>
    </div>
    <b-form-input
      v-if="show_Input"
      v-model="input_text"
      style="height: 13%"
      autocomplete="off"
      spellcheck="false"
      autofocus
    ></b-form-input>
  </div>
</template>
<script>
export default {
  name: 'ChatSystem',
  data() {
    return {
      show_Input: false,
      Shoe_Time: false,
      input_text: '',
      chats: [],
      Memory: [],
      indexMemory: 0,
    }
  },
  unmounted() {
    if ('alt' in window) {
    }
  },
  async mounted() {
    if ('alt' in window) {
    }
    await this.gotoZeroDownScroll()
    addEventListener('keyup', async (e) => {
      switch (e.key) {
        case 't':
          this.OpenChat()
          break
        case 'Enter':
          if (this.input_text == '') return this.CloseChat()
          await this.addchat(Date.now(), this.input_text)
          await this.AddtoMemorySystem(this.input_text)
          await this.CloseChat()
          break
        case 'Escape':
          this.CloseChat()
          break
        case 'ArrowUp':
          this.GetDataFromMemory(true)
          break
        case 'ArrowDown':
          this.GetDataFromMemory(false)
          break
        default:
          break
      }
    })
  },
  methods: {
    colorify(text) {
      let matches = []
      let m = null
      let curPos = 0
      do {
        m = /\{#[A-Fa-f0-9]{3}\}|\{#[A-Fa-f0-9]{6}\}/g.exec(text.substr(curPos))
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
            (i != 0 ? '</span>' : '') + '<span style="color: ' + color + '">'
          text =
            text.slice(0, matches[i].index) +
            insertHtml +
            text.slice(matches[i].index + matches[i].found.length, text.length)
        }
      }
      return text.replaceAll('\n', '<br>')
    },
    async addchat(TIME_SERVER, text) {
      let TimeStamp = new Date(TIME_SERVER)
      let Time = `${this.addziro(TimeStamp.getHours())}:${this.addziro(
        TimeStamp.getMinutes()
      )}:${this.addziro(TimeStamp.getSeconds())}`
      await this.pushMessage({ time: Time, chat: text })
      await this.gotoZeroDownScroll()
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
    OpenChat() {
      this.show_Input = true
    },
    CloseChat() {
      this.show_Input = false
      this.input_text = ''
    },
    GetDataFromMemory(up) {
      if (up) {
        if (this.indexMemory - 1 < 0) return
        this.indexMemory--
        this.input_text = this.Memory[this.indexMemory]
      } else {
        if (this.Memory[this.indexMemory] == undefined) return
        this.indexMemory++
        if (this.Memory[this.indexMemory] == undefined)
          return (this.input_text = '')
        this.input_text = this.Memory[this.indexMemory]
      }
    },
    AddtoMemorySystem(text) {
      if (this.Memory.length > 20) this.Memory.shift()
      this.Memory.push(text)
      this.indexMemory = this.Memory.length
    },
  },
}
</script>
<style scoped>
#Main_Chat {
  height: 100% !important;
}
#Chat_Display.activeScroll {
  overflow-y: scroll;
}
#Chat_Display {
  height: 85%;
  text-align: left;
  padding-left: 2%;
  overflow-wrap: break-word;
}
#Chat_Display {
  overflow-y: hidden;
}
#Chat_Display p {
  margin: 0 !important;
}
/* width */
::-webkit-scrollbar {
  width: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  box-shadow: inset 0 0 5px grey;
  border-radius: 10px;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: #999;
  border-radius: 10px;
}

.canSelectText span {
  -webkit-user-select: text !important; /* Safari */
  -ms-user-select: text !important; /* IE 10 and IE 11 */
  user-select: text !important; /* Standard syntax */
}

* {
  -webkit-user-select: none; /* Safari */
  -ms-user-select: none; /* IE 10 and IE 11 */
  user-select: none; /* Standard syntax */
}
</style>
