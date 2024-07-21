<script setup lang="ts">
import Header from './components/Header.vue'
import Cursor from './components/Cursor.vue'
</script>

<script lang="ts">
export default {
  data() {
    return {
      cursorClass: '',
      cursorX: 0,
      cursorY: 0,
      cursorHeight: 32,
      cursorWidth: 32,
      bgoffsetx: 0,
      bgoffsety: 0
    }
  },
  methods: {
    txtHover(event: any) {
      this.cursorClass = 'hover-text'
      this.cursorHeight = parseInt(window.getComputedStyle(event.target).fontSize, 10) * 1.5
    },

    btnHover(event: any) {
      this.cursorClass = 'hover'

      var rect = event.target.getBoundingClientRect()
      this.cursorHeight = rect.bottom - rect.top
      this.cursorWidth = rect.right - rect.left
      this.cursorX = (rect.left + rect.right) / 2 + window.scrollX
      this.cursorY = (rect.top + rect.bottom) / 2 + window.scrollY
      event.target.classList.add('hover')
    },

    btnRelease(event: any) {
      event.target.firstChild.style.transform = ''
      event.target.classList.remove('hover')
    },

    onLeave() {
      this.cursorClass = ''
      this.cursorHeight = 32
      this.cursorWidth = 32
    },

    updateMousePosition(event: any) {
      this.bgoffsetx = (event.clientX / window.innerWidth) * 50
      this.bgoffsety = (event.clientY / window.innerHeight) * 50

      if (this.cursorClass == 'hover') {
        var rect: DOMRect = event.target.getBoundingClientRect()
        var translateX = ((event.clientX - rect.left) / this.cursorWidth - 0.5) * 20
        var translateY = ((event.clientY - rect.top) / this.cursorHeight - 0.5) * 20
        event.target.firstChild.style.transform =
          'translate(' + translateX + '%, ' + translateY + '%)'
      } else {
        this.cursorX = event.clientX
        this.cursorY = event.clientY
      }
    }
  }
}
</script>

<template>
  <div id="app" @mousemove="updateMousePosition">
    <Cursor
      :x="cursorX"
      :y="cursorY"
      :cursorClass="cursorClass"
      :cursorHeight="cursorHeight"
      :cursorWidth="cursorWidth"
    ></Cursor>
    <Header
      @txthover="txtHover"
      @btnhover="btnHover"
      @btnrelease="btnRelease"
      @mouseleave="onLeave"
    ></Header>
  </div>
  <div
    id="bg"
    :style="{
      '--offsetx': bgoffsetx + 'px',
      '--offsety': bgoffsety + 'px'
    }"
  ></div>
</template>

<style scoped>
#app {
  pointer-events: all;
}

#bg {
  position: absolute;
  width: 100vw;
  height: 100vh;
  top: 0;
  left: 0;
  overflow: hidden;
}

#bg::after {
  content: '';
  position: absolute;
  overflow: hidden;
  width: calc(200vw + 200vh);
  height: calc(200vw + 200vh);
  top: calc(-100vw - 100vh);
  left: calc(-100vw - 100vh);
  z-index: -999;
  left: 0;

  background: url(./assets/icons/tile.webp) repeat 0 0;
  transition: all 0.1s ease-out;
  animation: blink 5s ease-in-out infinite;

  transform-origin: 0 0;
  transform: translate(var(--offsetx), var(--offsety)) rotate(45deg);
}

@keyframes blink {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0.6;
  }
  100% {
    opacity: 1;
  }
}

@media not (pointer: fine) {
  @keyframes slide {
    0% {
      transform: translate(0, 0) rotate(45deg);
    }
    100% {
      transform: translate(calc(sqrt(2) * 50px), calc(sqrt(2) * 50px)) rotate(45deg);
    }
  }

  #bg::after {
    animation:
      blink 5s ease-in-out infinite,
      slide 1s linear infinite;
  }
}
</style>
