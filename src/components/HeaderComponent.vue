// eslint-disable-next-line vue/multi-word-component-names
<script lang="ts">
export default {
  data() {
    return {
      birthDate: new Date(2004, 8, 16),
      age: 0
    }
  },
  emits: ['updateCursor'],
  methods: {
    updateCursor(event: Event) {
      this.$emit('updateCursor', event)
    },
    calculateAge() {
      const today = new Date()
      this.age = today.getFullYear() - this.birthDate.getFullYear()
      const monthDifference = today.getMonth() - this.birthDate.getMonth()
      const dayDifference = today.getDate() - this.birthDate.getDate()

      if (monthDifference < 0 || (monthDifference === 0 && dayDifference < 0)) {
        this.age--
      }
    }
  },
  created() {
    this.calculateAge()
  },
  mounted() {
    console.log('mounted 1')
  }
}
</script>

<template>
  <div class="header">
    <div class="left">
      <div class="top">
        <h3 @mouseover="updateCursor" @mouseleave="updateCursor">hey, my name is</h3>
        <h1 @mouseover="updateCursor" @mouseleave="updateCursor">Roland</h1>
      </div>
      <div class="center">
        <p @mouseover="updateCursor" @mouseleave="updateCursor">I'm a {{ age }}-year-old Computer Science student at Babeș-Bolyai University. I’m passionate about both backend and frontend development and love learning new technologies. I’m dedicated to solving problems and persist until I achieve success.</p>
      </div>
      <div class="bottom">
        <button tabindex="1" @mouseover="updateCursor" @mouseleave="updateCursor" onclick="window.location.href = 'https://github.com/prxbdead'">
          <div>
            <img alt="github-svg" src="@/assets/icons/github.svg" />
            github
          </div>
        </button>
      </div>
    </div>
    <div class="right"></div>
  </div>
  <div id="bg" ref="bg"></div>
</template>

<style>
.header {
  display: flex;
  justify-content: center;
  align-items: center;

  height: 100vh;
  width: 100%;
  padding: 10vh 10vw;

  .right {
    min-width: 30%;
  }

  .left {
    height: 100%;

    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: left;

    .top {
      justify-content: end;
      padding-bottom: 5vh;

      h1 {
        font-size: min(10vh, 20vw);
        font-weight: bolder;
        transform: translate(-0.08em, 0);
      }

      h3 {
        font-size: min(3vh, 6vw);
        font-weight: 100;
      }
    }

    .center {
      justify-content: center;

      p {
        font-weight: normal;
        font-size: min(2vh, 4vw);
      }
    }

    .bottom {
      justify-content: first;
      padding: 5vh 0;
    }
  }
}

@media screen and (max-width: 800px) {
  .header {
    .right {
      display: none;
    }
  }
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

  background: url(../assets/icons/tile.webp) repeat 0 0;
  transition: all 0.1s ease-out;
  animation: blink 5s ease-in-out infinite;

  transform-origin: 0 0;
  transform: translate(var(--bgoffsetx), var(--bgoffsety)) rotate(45deg);
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
