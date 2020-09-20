<template>
  <div id="app">
    <LolCard summoner="18Hnxe8uIW0U8R5joJfle4GLdLTN7ym9qI-9It10HfBwfQ4" API="RGAPI-55b86d13-f0c5-446f-98f7-570e2b9fcc82"/>
  </div>
</template>

<script>
import LolCard from './components/LolCard.vue'

export default {
  name: 'App',
  components: {
    LolCard
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
