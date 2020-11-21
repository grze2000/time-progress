<template>
  <div class="control">
    <div class="control__header">
      <h1 class="control__title">TimeProgress</h1>
      <a :class="['control__show-btn', `control__show-btn--${open ? 'on' : 'off'}`]" @click="open = !open"></a>
    </div>
    <transition name="toggle">
      <div class="control__content" v-if="open">
        Hello content
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'ProgressControl',
  data() {
    return {
      open: false
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
  }

  &__show-btn {
    margin-left: auto;
    border-left: 1em solid transparent;
    border-right: 1em solid transparent;
    cursor: pointer;

    &--on {
      border-bottom: 1em solid black;
    }

    &--off {
      border-top: 1em solid black;
    }
  }

  &__title {
    margin: 0;
    font-size: 1.5em;
  }
}
.toggle-enter-active, .toggle-leave-active {
  transition: max-height 1s;
  overflow: hidden;
  max-height: 100px;
}
.toggle-enter, .toggle-leave-to {
  max-height: 0;
}
</style>
