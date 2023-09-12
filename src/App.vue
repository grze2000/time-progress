<template>
  <div class="app">
    <ProgressControl @add="addProgressbar"></ProgressControl>
    <ProgressBox v-for="progress in progressbarList"
      :key="progress.timestamp"
      :progress="progress"
      :date="date"
      @delete="deleteProgressbar(progress)"
      @toggleLocked="progress.locked = !progress.locked"
    ></ProgressBox>
  </div>
</template>

<script>
import ProgressBox from './components/ProgressBox.vue'
import ProgressControl from './components/ProgressControl.vue'

export default {
  name: 'App',
  components: {
    ProgressBox,
    ProgressControl
  },
  data() {
    return {
      progressbarList: [],
      date: new Date()
    }
  },
  created() {
    this.timer = setInterval(() => {
      this.date = new Date();
    }, 1000);
    this.progressbarList = (JSON.parse(localStorage.getItem('progressbarList')) || []).map(x => {
      x.endTime = new Date(x.endTime);
      x.startTime = new Date(x.startTime);
      return x;
    });
  },
  methods: {
    addProgressbar(progress) {
      this.progressbarList.push(progress);
      localStorage.setItem('progressbarList', JSON.stringify(this.progressbarList));
    },
    deleteProgressbar(progress) {
      this.progressbarList.splice(this.progressbarList.findIndex(x => x.timestamp === progress.timestamp), 1);
      localStorage.setItem('progressbarList', JSON.stringify(this.progressbarList));
    }
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
  box-sizing: border-box;
}
</style>
