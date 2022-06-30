<template>
<div class="chat theme-lysnake">
  <div v-for="msg in msgViewer" v-bind:key="msg.id">
    <Message :msgViewer='msg' :colorBadge="msg.colorBadge" :logoBadge="msg.logoBadge"/>
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

      if (msg.badges?.broadcaster) {
        msg.colorBadge = "color: #ff0000";
        msg.logoBadge = "https://static-cdn.jtvnw.net/badges/v1/5527c58c-fb7d-422d-b71b-f309dcb85cc1/1";
      } else if (msg.badges?.moderator) {
        msg.colorBadge = "color: #00AD03";
        msg.logoBadge = "https://static-cdn.jtvnw.net/badges/v1/3267646d-33f0-4b17-b3df-f923a41db1d0/1";
      } else if (msg.badges?.vip) {
        msg.colorBadge = "color: #E005B9";
        msg.logoBadge = "https://static-cdn.jtvnw.net/badges/v1/b817aba4-fad8-49e2-b88a-7cc744dfa6ec/1";
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
  height: calc(100vh - 7vh);
  padding: 0;
  margin: 0;
  background: transparent;
  font-family: "Montserrat", sans-serif;
  color: white;
  line-height: 1;
}

html, body, div, span, applet, object, iframe, h1, h2, h3, h4, h5, h6, p, blockquote, pre, a, abbr, acronym, address, big, cite, code, del, dfn, em, img, ins, kbd, q, s, samp, small, strike, strong, sub, sup, tt, var, b, u, i, center, dl, dt, dd, ol, ul, li, fieldset, form, label, legend, table, caption, tbody, tfoot, thead, tr, th, td, article, aside, canvas, details, embed, figure, figcaption, footer, header, hgroup, menu, nav, output, ruby, section, summary, time, mark, audio, video {
    margin: 0;
    padding: 0;
    border: 0;
    font-size: 100%;
    font: inherit;
    vertical-align: baseline;
}

.theme-lysnake {
  font-family: "Montserrat", sans-serif;
  margin-bottom: 32px;
  height: 100%;
  max-height: 100vh;
  flex-direction: column;
  align-items: flex-end;
  justify-content: flex-end;
  display: flex;
  padding: 32px;
}
</style>
