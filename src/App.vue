<script setup lang="ts">
import { ref } from 'vue'
import Header from './components/Header.vue'
import Cursor from './components/Cursor.vue'

const bg = ref<HTMLElement | null>(null)
</script>

<script lang="ts">
export default {
  data() {
    return {
      cursorClass: '',
      cursorX: 0,
      cursorY: 0,
      cursorHeight: 32,
      cursorWidth: 32
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
      this.cursorX = (rect.left + rect.right) / 2
      this.cursorY = (rect.top + rect.bottom) / 2
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
      if (bg.value) {
        var rect: DOMRect = bg.value.getBoundingClientRect()
        bg.value.style.backgroundPosition =
          (event.clientX / rect.right) * 50 + 'px ' + (event.clientY / rect.bottom) * 50 + 'px'
      }
      if (this.cursorClass == 'hover') {
        rect = event.target.getBoundingClientRect()
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
    <div id="bg" ref="bg"></div>
  </div>
</template>

<style scoped>
#app {
  pointer-events: all;
}

#bg {
  position: absolute;
  top: 0;
  width: 100vw;
  height: 100vh;
  z-index: -999;
  background: url(./assets/icons/tile.png) repeat 0 0;
  transition: background-position 0.1s ease-out;
  opacity: 0.3;
}
</style>
