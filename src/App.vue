<script setup lang="ts">
import Header from './components/HeaderComponent.vue'
import Cursor from './components/CursorComponent.vue'
</script>

<script lang="ts">
export default {
  data() {
    return {
      cursor: {
        class: '',
        x: 0,
        y: 0,
        h: 32,
        w: 32
      },
      page: 0
    }
  },
  methods: {
    updateCursor(event: Event) {
      const target = event.target as HTMLElement
      if (!target.childElementCount) {
        if (event.type === 'mouseover') {
          this.cursor.class = 'hover-text'
          this.cursor.h = parseInt(window.getComputedStyle(target).fontSize, 10) * 1.5
        } else {
          this.cursor.class = ''
          this.cursor.h = 32
          this.cursor.w = 32
        }
      } else {
        if (event.type === 'mouseover') {
          this.cursor.class = 'hover'
          var rect = target.getBoundingClientRect()
          this.cursor.h = rect.bottom - rect.top
          this.cursor.w = rect.right - rect.left
          this.cursor.x = (rect.left + rect.right) / 2 + window.scrollX
          this.cursor.y = (rect.top + rect.bottom) / 2 + window.scrollY
          target.classList.add('hover')
        } else {
          this.cursor.class = ''
          this.cursor.h = 32
          this.cursor.w = 32
          target.classList.remove('hover')
        }
      }
    },
    updateMousePosition(event: any) {
      const h = document.querySelector(':root') as HTMLElement

      h.style.setProperty('--bgoffsetx', (event.clientX / window.innerWidth) * 50 + 'px')
      h.style.setProperty('--bgoffsety', (event.clientY / window.innerHeight) * 50 + 'px')

      if (this.cursor.class == 'hover') {
        const rect: DOMRect = event.target.getBoundingClientRect()
        event.target.firstChild.style.transform = 'translate(' + ((event.clientX - rect.left) / this.cursor.w - 0.5) * 20 + '%, ' + ((event.clientY - rect.top) / this.cursor.h - 0.5) * 20 + '%)'
      } else {
        this.cursor.x = event.clientX
        this.cursor.y = event.clientY
      }
    },
    onScroll(event: any) {
      event.preventDefault()
      let el: HTMLElement
      switch (this.page) {
        case 0:
          el = this.$refs.test as HTMLElement
          el.style.top = '0'
          this.page = 1
          break
        case 1:
          el = this.$refs.test as HTMLElement
          el.style.top = '0'
          this.page = 0
      }
    }
  }
}
</script>

<template>
  <div id="container" @mousemove="updateMousePosition" @wheel="onScroll" @touchmove="onScroll" @scroll.prevent>
    <Cursor :cursor="cursor"></Cursor>
    <Header v-if="page == 0" @updateCursor="updateCursor"></Header>
  </div>
</template>

<style scoped>
#container {
  max-height: 100vh;
  pointer-events: all;
  overflow-y: scroll;
}
</style>
