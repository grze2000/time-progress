<template>
  <div class="control">
    <div class="control__header">
      <h1 class="control__title">TimeProgress</h1>
      <a class="control__show-btn" @click="open = !open">
        <span :class="{'icon-down-open': !open, 'icon-up-open': open}"></span>
      </a>
    </div>
    <transition name="toggle">
      <div class="control__content" v-if="open">
        <div class="form">
          <date-picker v-model="progress.startTime" type="datetime" class="form__datepicker"></date-picker>
          <date-picker v-model="progress.endTime" type="datetime" class="form__datepicker"></date-picker>
          <input v-model="progress.title" type="text" placeholder="Tytuł" class="form__textfield">
          <input v-model="progress.description" type="text" placeholder="Opis" class="form__textfield">
          <input v-model="progress.color" type="color" class="form__colorpicker">
          <button class="form__submit-btn" @click="addProgressbar">Dodaj</button>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';

export default {
  name: 'ProgressControl',
  components: { DatePicker },
  data() {
    return {
      progress: {
        title: '',
        color: '#000000',
        description: '',
        startTime: null,
        endTime: null,
        locked: false
      },
      open: true
    }
  },
  methods: {
    addProgressbar() {
      this.$emit('add', { timestamp: (new Date()).getTime(), ...this.progress });
      this.progress = {
        title: '',
        color: '#000000',
        description: '',
        startTime: null,
        endTime: null
      }
    }
  }
}
</script>

<style lang="scss">
@import '../scss/mixins.scss';

.control {
  @include container;
  transition: all 1s;

  &__header {
    display: flex;
    flex-direction: row;
    align-items: center;
    font-size: 1.5em;
  }

  &__show-btn {
    margin-left: auto;
    cursor: pointer;
  }

  &__title {
    margin: 0;
    font-size: 1em;
  }
}
.toggle-enter-active, .toggle-leave-active {
  transition: max-height 1s;
  overflow: hidden;
  max-height: 100px;

  @media (max-width: 960px) {
    max-height: 300px;
  }
}
.toggle-enter, .toggle-leave-to {
  max-height: 0;
}
.form {
  display: grid;
  grid-template-columns: repeat(4, 1fr) auto 1fr;
  grid-gap: 10px;
  align-items: center;
  margin-top: 15px;

  @media (max-width: 960px) {
    grid-template-columns: none;
    grid-template-rows: repeat(6, 1fr);
  }

  &__datepicker {
    width: 100%;
  }

  &__textfield {
    @include input-border;
    padding: 9px;
    min-width: 0;
  }

  &__submit-btn {
    @include input-border;
    @include form-btn;
    padding: 9px;
    font-weight: bold;
  }

  &__colorpicker {
    @include input-border;
    @include form-btn;
    height: 100%;
    box-sizing: border-box;

    @media (max-width: 960px) {
      width: 100%;
    }
  }
}
</style>
