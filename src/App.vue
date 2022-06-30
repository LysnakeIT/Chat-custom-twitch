<template>
<div className="chat theme-lysnake">
  <div v-for="msg in msgViewer" v-bind:key="msg.id">
    <Message :msgViewer='msg' :colorBadge="this.colorBadge" :logoBadge="this.logoBadge"/>
  </div>
</div>
</template>


<script>
import tmi from "tmi.js";
import { parse } from "simple-tmi-emotes";
import Message from '@/components/Message.vue'

export default {
  name: 'Chat-twitch-LysnakeIT',
  components: {
    Message
  },
  data() {
    return {
      colorBadge: String,
      logoBadge: String,
      msgViewer: [],
    };
  },
  async mounted() {
    const client = new tmi.Client({ channels: ['LysnakeIT' || ""] });
    client.connect()

    client.on("message", (_, tags, message) => {
      const options = {
        format: "default",
        themeMode: "light",
        scale: "2.0",
      };

      const msg = {
        id: tags?.id,
        username: tags["display-name"],
        twitch: tags?.username,
        emotes: tags?.emotes || {},
        date: new Date(),
        message,
        badges: tags?.badges,
        mod: tags?.mod,
        subscriber: tags?.subscriber,
        color: tags?.color,
      };

      msg.message = parse(msg.message, msg.emotes, options);

      var tempMsg = msg

      this.colorBadge = "";
      this.logoBadge = "";
      if (tempMsg.badges?.broadcaster) {
        this.colorBadge = "color: #ff0000";
        this.logoBadge = "https://static-cdn.jtvnw.net/badges/v1/5527c58c-fb7d-422d-b71b-f309dcb85cc1/1";
      } else if (tempMsg.badges?.moderator) {
        this.colorBadge = "color: #00AD03";
        this.logoBadge = "https://static-cdn.jtvnw.net/badges/v1/3267646d-33f0-4b17-b3df-f923a41db1d0/1";
      } else if (tempMsg.badges?.vip) {
        this.colorBadge = "color: #E005B9";
        this.logoBadge = "https://static-cdn.jtvnw.net/badges/v1/b817aba4-fad8-49e2-b88a-7cc744dfa6ec/1";
      }
      this.msgViewer.push(msg);
    });

    client.on("connected", () => {
      console.log("Je suis bien connecté");
    });
  }
}

</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@700;900&display=swap");

#app {
  width: 100%;
  height: calc(100vh - 3vh);
  padding: 0;
  margin: 0;
}

.theme-lysnake {
  font-family: "Montserrat", sans-serif;
  margin-bottom: 32px;
  width: 100%;
  height: 100%;
  max-height: 100vh;
  flex-direction: column;
  align-items: flex-end;
  justify-content: flex-end;
  display: flex;
}
</style>
