<template>
  <div id="app">
    <LolCard summoner="" API="" />
  </div>
</template>

<script>
import LolCard from "./components/LolCard.vue";

export default {
  name: "App",
  components: {
    LolCard,
  },
};
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
