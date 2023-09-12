<template>
  <article :class="['progress', {'locked': progress.locked}]">
    <div class="progress__header">
      <div class="header__content">
        <h2 class="header__title" :style="{color: progress.color}">{{ progress.title }}</h2>
        <div class="header__description">
          <div class="header__description--active">{{ progress.description }}</div>
          <div class="header__description--inactive">{{ remainingTime ? remainingTime : 'Zakończono' }}</div>
        </div>
      </div>
      <div class="header__actions">
        <button class="btn btn--lock" @click="$emit('toggleLocked')"></button>
        <button class="btn btn--exit" @click="$emit('delete')"></button>
      </div>
    </div>
    <div class="progress__bar-wrapper">
      <div class="progress__bar">
        <span class="progress__time progress__time--start">{{ progress.startTime | dateTime }}</span>
        <span class="progress__time progress__time--end">{{ progress.endTime | dateTime }}</span>
        <span class="progress__time progress__time--remaining">{{ remainingTime ? `${progress.startTime > date ? 'Do rozpoczęcia:' : 'Pozostało:'} ${remainingTime}` : 'Zakończono' }}</span>
        <div class="progress__meter" :style="{'background-color': progress.color, width: `${value}%`}">
          <span class="progress__meter--text">{{ value }}%</span>
        </div>
      </div>
    </div>
  </article>
</template>

<script>
export default {
  name: 'ProgressBox',
  props: {
    progress: {
      timestamp: Number,
      title: {
        type: String,
        required: true,
        default: ''
      },
      description: {
        type: String,
        default: ''
      },
      startTime: {
        type: Date,
        required: true
      },
      endTime: {
        type: Date,
        required: true
      },
      color: {
        type: String,
        default: '#00ff00'
      },
      locked: Boolean
    },
    date: Date
  },
  methods: {
    calculateTimeBetweenDates(date1, date2) {
      const time = (date1.getTime() - date2.getTime()) / 1000;
      const seconds = Math.floor(time % 60);
      const minutes = Math.floor((time / 60) % 60);
      const hours = Math.floor(time / 3600 % 24);
      const days = Math.floor(time / 86400);
      return `${days > 0 ? days + ':' : ''}${hours > 9 ? hours : '0' + hours}:${minutes > 9 ? minutes : '0' + minutes}:${seconds > 9 ? seconds : '0' + seconds}`;
    }
  },
  computed: {
    remainingTime() {
      if(this.progress.startTime > this.date) {
        return this.calculateTimeBetweenDates(this.progress.startTime, this.date);
      } else if(this.progress.endTime > this.date) {
        return this.calculateTimeBetweenDates(this.progress.endTime, this.date);
      } else {
        return null;
      }
    },
    value() {
      const totalTime = (this.progress.endTime - this.progress.startTime) / 1000;
      const time = (this.date - this.progress.startTime) / 1000;
      return time < 0 ? 0 : time > totalTime ? 100 : (time * 100 / totalTime).toFixed(2);
    }
  },
  filters: {
    dateTime(date) {
      const date1 = (new Date(date.getTime())).setHours(0, 0, 0, 0);
      const date2 = (new Date(date.getTime())).setHours(0, 0, 0, 0);
      return date1 === date2 ? date.toLocaleTimeString() : date.toLocaleString();
    }
  }
}
</script>

<style lang="scss">
@import '../scss/mixins.scss';

.progress {
  @include container;

  font-size: 0.9rem;
  font-weight: bold;
  color: #a8a8a8;

  &__header {
    display: flex;
    flex-direction: row;
  }

  &__bar-wrapper {
    transition: all 1s;
    margin-top: 0px;
    position: relative;
  }

  &__bar {
    width: 100%;
    background-color: #f0f0f0;
    overflow: hidden;
    border-radius: 15px;
    transition: all 1s;
  }

  &__meter {
    padding: 5px 0;
    box-sizing: border-box;
    text-align: center;
    overflow: hidden;
    max-height: 10px;
    transition: all 1s;
  }

  &__meter--text {
    visibility: hidden;
    opacity: 0;
    font-size: 0px;
    transition: all 1s;
  }

  &__time {
    position: absolute;
    top: calc(-1em - 5px);
    transition: all 1s;
    visibility: hidden;
    opacity: 0;

    &--start {
      left: 5px;
    }

    &--end {
      right: 5px;
    }

    &--remaining {
      left: 50%;
      transform: translateX(-50%);
    }
  }
}
.progress:hover, .progress.locked {
  .progress__bar-wrapper {
    margin-top: 30px;
  }

  .progress__time {
    visibility: visible;
    opacity: 1;
  }

  .progress__meter--text {
    visibility: visible;
    opacity: 1;
    font-size: 1em;
  }

  .progress__meter {
    max-height: 30px;
  }
  .header__description--active {
    transform: none;
    visibility: visible;
    opacity: 1;
    min-height: 1em;
  }
  .header__description--inactive {
    transform: none;
    visibility: hidden;
    opacity: 0;
  }
}
.header {
  &__title {
    margin: 0;
    padding: 0;
  }

  &__description {
    padding: 0;
    margin: 10px 0;
    height: 1.2em;
    overflow: hidden;

    &--active,
    &--inactive {
      transform: translateY(-100%);
      transition: all 1s;
    }

    &--active {
      visibility: hidden;
      opacity: 0;
      min-height: 1em;
    }

    &--inactive {
      visibility: visible;
      opacity: 1;
    }
  }

  &__actions {
    margin-left: auto;

    .btn:not(:last-child) {
      margin-right: 5px;
    }
  }
}
.btn {
  border-radius: 50%;
  background-color: #f0f0f0;
  border: 0;
  width: 30px;
  height: 30px;
  position: relative;
  cursor: pointer;
  padding: 2px;

  &:hover {
    background-color: #dfdfdf;
  }

  &:focus {
    border: 2px solid #8d8d8d;
    outline: 0;
    padding: 0;
  }

  &::before {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-family: "fontello";
    font-size: 14px;
  }

  &--exit:before {
    content: "\e801";
  }

  &--lock:before {
    content: "\e800";
  }
}
.progress.locked .btn--lock:before {
  content: "\f13e";
}
</style>
