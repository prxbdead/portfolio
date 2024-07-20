<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import { ref, onMounted, onUnmounted } from 'vue'

import HelloWorld from './components/HelloWorld.vue'

const startMenu = ref('')
const currentTime = ref(new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }))
const apps = ref([
  { name: 'teeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeest' },
  { name: 'asdfaaaaaaaaaafasfadafsafasfa' },
  { name: '3' },
  { name: '4' },
  { name: '5' },
  { name: '6' }
])

const activeApp = ref(apps.value[0])

const updateCurrentTime = () => {
  currentTime.value = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
}

let intervalId: number | undefined

onMounted(() => {
  intervalId = setInterval(updateCurrentTime, 1000)
})

onUnmounted(() => {
  clearInterval(intervalId)
})
</script>

<template>
  <!--
  <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />

      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />-->
  <footer>
    <div class="container">
      <div id="left">
        <input type="checkbox" id="startMenu" v-model="startMenu" />
        <label class="btn btn-startmenu" :class="{ active: startMenu }" for="startMenu">
          <span><img src="./assets/icons/startmenu.png" draggable="false" /></span>
          <span>Start</span>
        </label>
      </div>
      <div id="center">
        <div
          class="btn btn-open"
          :class="{ active: app == activeApp && !startMenu }"
          :style="{ width: 'calc((100vw - 174px) / ' + apps.length + ')', 'max-width': '10vw' }"
          v-for="app in apps"
          @click="(activeApp = app), (startMenu = '')"
        >
          <img src="./assets/icons/startmenu.png" draggable="false" />
          <span>
            {{ app.name }}
          </span>
        </div>
      </div>
      <div id="right">
        <div class="btn btn-time">
          <span> {{ currentTime }} </span>
        </div>
      </div>
    </div>
  </footer>
</template>
