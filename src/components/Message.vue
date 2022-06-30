<template>
<section class="Terminal">
  <div class="Terminal_boiteTerminal">
    <div class="Terminal_boutonVert"></div>
    <div class="Terminal_boutonJaune"></div>
    <div class="Terminal_boutonRouge"></div>
    <div class="Terminal_boiteDsBoite">
      <h1 class="Terminal_title" :style="colorBadge">{{msgViewer.username}}
        <img class="Terminal_title_badge" :src="logoBadge">
      </h1>
      <hr class="Terminal_barreTerminal">
      <div class="message">
        <div v-html="msgViewer.message"></div>
      </div>
    </div>
  </div>
</section>
</template>


<script>
export default {
  name: 'Chat-message',
  props: {
    msgViewer: Object,
    colorBadge: String,
    logoBadge: String
  }
}
</script>

<style>
@keyframes pop {
  0% {
    opacity: 0;
    left: 110%;
  }

  100% {
    opacity: 1;
    left: 0;
  }
}

.Terminal {
  background-color: #1C152D;
  margin-top: 30px;
  border-radius: 15px;
  animation: pop 1s cubic-bezier(1, 0, 0, 1) both;
}

.Terminal_boiteTerminal {
  position: relative;
  max-width: 100%;
  min-width: 525px;
  background: rgba(40,40,40,.3);
  border: 1px solid #000;
  border-radius: 15px;
}

.Terminal_boutonVert, .Terminal_boutonJaune {
  position: absolute;
  width: 13px;
  height: 13px;
  top: 10%;
  border-radius: 500px;
}

.Terminal_boutonVert {
  right: 75px;
  background: #8fff00;
}

.Terminal_boutonJaune {
  right: 50px;
  background: #ff8a00;
}

.Terminal_boutonRouge {
  position: absolute;
  width: 13px;
  height: 13px;
  right: 25px;
  top: 10%;
  background: #ba2c2c;
  border-radius: 500px;
}

.Terminal_boiteDsBoite {
  top: 70px;
  border-radius: 15px;
}

.Terminal_title {
  font-style: normal;
  font-weight: 520;
  font-size: 20px;
  line-height: 2.2;
  text-align: center;
  color: white;
}

.Terminal_title_badge {
  height: auto;
  vertical-align: calc(10% - 6px);
  margin: 0 4px;
  border-radius: 50%;
  width: unset !important;
}

.Terminal_barreTerminal {
  max-width: 100%;
  height: 0;
  left: 0;
  top: -15px;
  z-index: 1;
  border: 1px solid #000;
}

.message {
  padding: 16px;
  color: white;
  font-size: 14px;
  text-align: right;
  font-weight: 500;
  overflow-wrap: anywhere;
  max-width: 100%;
  vertical-align: baseline;
  line-height: 1.4;
}

img {
  width: 24px;
  height: auto;
  border-radius: 50%;
  vertical-align: middle !important;
  margin: 0 4px !important;
}
</style>
