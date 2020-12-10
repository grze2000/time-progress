<template>
  <div class="app">
    <ProgressControl @add="progressbarList.push($event)"></ProgressControl>
    <Progress v-for="progress in progressbarList" :key="progress.title" :progress="progress" :date="date"></Progress>
  </div>
</template>

<script>
import Progress from './components/Progress.vue'
import ProgressControl from './components/ProgressControl.vue'

export default {
  name: 'App',
  components: {
    Progress,
    ProgressControl
  },
  data() {
    return {
      progressbarList: [],
      date: null
    }
  },
  created() {
    this.timer = setInterval(() => {
      this.date = new Date();
    }, 1000);
  }
}
</script>

<style lang="scss">
html, body {
  height: 100%;
}
body {
  margin: 0;
}
.app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background-color: #E5E5E5;
  height: 100%;
  padding: 15px 30px;
}
</style>
