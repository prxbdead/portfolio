<script lang="ts">
export default {
  props: {
    cursorHeight: {
      type: Number,
      default: 32
    },
    cursorWidth: {
      type: Number,
      default: 32
    },
    cursorClass: {
      type: String,
      default: ''
    },
    x: {
      type: Number,
      required: true
    },
    y: {
      type: Number,
      required: true
    }
  },
  computed: {
    cursorStyle() {
      return {
        top: `${this.y}px`,
        left: `${this.x}px`,
        height: `${this.cursorHeight}px`,
        width: `${this.cursorWidth}px`
      }
    }
  }
}
</script>

<template>
  <div id="cursor" :class="[cursorClass]" :style="cursorStyle"></div>
</template>

<style scoped>
@media not (pointer: fine) {
  #cursor {
    display: none;

    &.hover-text {
      display: block;
    }
  }
}

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
</style>
