// eslint-disable-next-line vue/multi-word-component-names
<script lang="ts">
export default {
  methods: {
    txtHover(event: any) {
      this.$emit('txthover', { target: event.target })
    },
    btnHover(event: any) {
      this.$emit('btnhover', { target: event.target })
    },
    btnRelease(event: any) {
      this.$emit('btnrelease', { target: event.target })
    },
    calculateAge() {
      const today = new Date()
      let age = today.getFullYear() - this.birthDate.getFullYear()
      const monthDifference = today.getMonth() - this.birthDate.getMonth()
      const dayDifference = today.getDate() - this.birthDate.getDate()

      if (monthDifference < 0 || (monthDifference === 0 && dayDifference < 0)) {
        age--
      }

      this.age = age
    },
    redirGithub() {
      window.location.href = 'https://github.com/prxbdead'
    }
  },
  created() {
    this.calculateAge()
  },
  data: function () {
    return {
      birthDate: new Date(2004, 8, 16),
      age: 0
    }
  }
}
</script>

<template>
  <div class="header">
    <div class="left">
      <div class="top">
        <h3 @mouseover="txtHover">hey, my name is</h3>
        <h1 @mouseover="txtHover">Roland</h1>
      </div>
      <div class="center">
        <p @mouseover="txtHover">
          I'm a {{ age }}-year-old Computer Science student at Babeș-Bolyai University. I’m
          passionate about both backend and frontend development and love learning new technologies.
          I’m dedicated to solving problems and persist until I achieve success.
        </p>
      </div>
      <div class="bottom">
        <button @mouseover="btnHover" @mouseleave="btnRelease" @click="redirGithub">
          <div>
            <img alt="github-svg" src="@/assets/icons/github.svg" />
            github
          </div>
        </button>
      </div>
    </div>
    <div class="right"></div>
  </div>
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
</style>
