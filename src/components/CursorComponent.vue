<script lang="ts">
export default {
  props: {
    cursor: {
      type: Object,
      default: () => ({
        class: '',
        x: 0,
        y: 0,
        h: 32,
        w: 32
      })
    }
  },
  computed: {
    cursorStyle() {
      return {
        top: `${this.cursor.y}px`,
        left: `${this.cursor.x}px`,
        height: `${this.cursor.h}px`,
        width: `${this.cursor.w}px`
      }
    }
  }
}
</script>

<template>
  <div id="cursor" :class="[cursor.class]" :style="cursorStyle"></div>
</template>

<style scoped>
#cursor {
  display: block;
  transform: translate(-50%, -50%);
  position: absolute;
  border-radius: 50%;
  pointer-events: none;
  transition: all 0.1s ease-out;
  background: #2e2933;
  opacity: 0.8;
  z-index: 999;
}

#cursor.hover {
  border-radius: 3em;
  z-index: 0;
  transition: all 0.1s ease-out;
}

#cursor.hover-text {
  border-radius: 32px;
  width: 4px !important;
  background: #7f00ff;
  left: 0;
}

@media not (pointer: fine) {
  #cursor {
    display: none;

    &.hover {
      display: block;
    }
  }
}
</style>
