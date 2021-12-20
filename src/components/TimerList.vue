<template>
  <section v-if="timers">
    <header>
      <h2>{{ headerText }}</h2>
    </header>
    <article v-for="timer in timers" :key="timer">
      <div class="timer" v-if="new Date(timer) > currentDate">
        <div class="timer__chosen-date">
          {{ chosenDateText }} {{ new Date(timer).toLocaleString() }}
        </div>
        <div class="timer__countdown">
          {{ leftTimeText }}
          <!-- days -->
          {{ diffDays(timer) }}
          <span v-if="diffDays(timer) === 1">{{ dayText }}</span
          ><span v-if="diffDays(timer) > 1 || diffDays(timer) === 0">{{
            daysText
          }}</span>
          <!-- hours -->
          {{ diffHours(timer) }}

          <span v-if="diffHours(timer) === 1">{{ hourText }}</span>
          <span v-if="diffHours(timer) > 1 && diffHours(timer) < 5">{{
            lessThan5HoursText
          }}</span>
          <span v-if="diffHours(timer) >= 5 || diffHours(timer) === 0">{{
            over5HoursText
          }}</span>

          <span v-if="diffMinutes(timer) < 10">0</span>{{ diffMinutes(timer) }}
          <!-- minutes -->

          <span v-if="diffMinutes(timer) === 1">{{ minuteText }}</span>
          <span v-if="diffMinutes(timer) > 1 && diffMinutes(timer) < 5">{{
            lessThan5MinutesText
          }}</span>
          <span v-if="diffMinutes(timer) >= 5 || diffMinutes(timer) === 0">{{
            over5MinutesText
          }}</span>

          <span v-if="diffSeconds(timer) < 10">0</span>{{ diffSeconds(timer) }}

          <!-- seconds -->
          <span v-if="diffSeconds(timer) === 1">{{ secondText }}</span>
          <span v-if="diffSeconds(timer) > 1 && diffSeconds(timer) < 5">{{
            lessThan5SecondsText
          }}</span>
          <span v-if="diffSeconds(timer) >= 5 || diffSeconds(timer) === 0">{{
            over5SecondsText
          }}</span>
        </div>
        <button @click="handleDelete(timer)" class="timer__delete-button">
          {{ buttonText }}
        </button>
      </div>
    </article>
  </section>
</template>

<script>
export default {
  name: 'TimerList',
  props: ['chosenLanguage'],
  data() {
    return {
      currentDate: new Date(),
      timers: [],
      calculatedTime: {
        milisecondsInDay: 1000 * 60 * 60 * 24,
        milisecondsInHour: 1000 * 60 * 60,
        milisecondsInMinute: 1000 * 60,
        milisecondsInSecond: 1000,
        hoursInDay: 24,
        minutesInHour: 60,
        secondsInMinute: 60,
      },
    };
  },
  methods: {
    diffDays(timer) {
      return Math.floor(
        (Date.parse(timer) - this.currentDate) /
          this.calculatedTime.milisecondsInDay
      );
    },

    diffHours(timer) {
      return (
        Math.floor(
          (Date.parse(timer) - this.currentDate) /
            this.calculatedTime.milisecondsInHour
        ) % this.calculatedTime.hoursInDay
      );
    },

    diffMinutes(timer) {
      return (
        Math.floor(
          (Date.parse(timer) - this.currentDate) /
            this.calculatedTime.milisecondsInMinute
        ) % this.calculatedTime.minutesInHour
      );
    },

    diffSeconds(timer) {
      return (
        Math.floor(
          (Date.parse(timer) - this.currentDate) /
            this.calculatedTime.milisecondsInSecond
        ) % this.calculatedTime.secondsInMinute
      );
    },

    handleDelete(item) {
      this.timers = this.timers.filter((timer) => {
        return timer !== item;
      });
      localStorage.setItem('timers', JSON.stringify(this.timers));
    },
  },

  computed: {
    headerText() {
      if (this.chosenLanguage === 'polish') {
        return 'Aktywne liczniki';
      } else {
        return 'Active timers';
      }
    },
    buttonText() {
      if (this.chosenLanguage === 'polish') {
        return 'Usuń licznik';
      } else {
        return 'Delete the timer';
      }
    },
    chosenDateText() {
      if (this.chosenLanguage === 'polish') {
        return 'Wybrana data';
      } else {
        return 'Chosen date';
      }
    },
    leftTimeText() {
      if (this.chosenLanguage === 'polish') {
        return 'Do wybranej daty pozostało: ';
      } else {
        return 'To chosen date left: ';
      }
    },
    dayText() {
      if (this.chosenLanguage === 'polish') {
        return 'dzień ';
      } else {
        return 'day ';
      }
    },
    daysText() {
      if (this.chosenLanguage === 'polish') {
        return 'dni ';
      } else {
        return 'days ';
      }
    },
    hourText() {
      if (this.chosenLanguage === 'polish') {
        return ' godzina ';
      } else {
        return ' hour ';
      }
    },
    lessThan5HoursText() {
      if (this.chosenLanguage === 'polish') {
        return ' godziny ';
      } else {
        return ' hours ';
      }
    },
    over5HoursText() {
      if (this.chosenLanguage === 'polish') {
        return ' godzin ';
      } else {
        return ' hours ';
      }
    },

    minuteText() {
      if (this.chosenLanguage === 'polish') {
        return ' minuta ';
      } else {
        return ' minute ';
      }
    },
    lessThan5MinutesText() {
      if (this.chosenLanguage === 'polish') {
        return ' minuty ';
      } else {
        return ' minutes ';
      }
    },
    over5MinutesText() {
      if (this.chosenLanguage === 'polish') {
        return ' minut ';
      } else {
        return ' minutes ';
      }
    },
    secondText() {
      if (this.chosenLanguage === 'polish') {
        return ' sekunda';
      } else {
        return ' second';
      }
    },
    lessThan5SecondsText() {
      if (this.chosenLanguage === 'polish') {
        return ' sekundy';
      } else {
        return ' seconds';
      }
    },
    over5SecondsText() {
      if (this.chosenLanguage === 'polish') {
        return ' sekund';
      } else {
        return ' seconds';
      }
    },
  },

  mounted() {
    this.timers = JSON.parse(localStorage.getItem('timers'));
    setInterval(() => {
      this.currentDate = new Date();
    }, 1000);
  },
};
</script>

<style lang="scss">
@import '../colors';

.timer {
  background: rgb(255, 255, 255);
  margin: 1rem 0;
  max-width: 40rem;
  min-width: 35rem;
  padding: 1rem 0 0.3rem;
  border-bottom: 1px dotted $dark-text-color;

  &__chosen-date {
    font-size: 1.2rem;
    font-weight: 500;
  }

  &__countdown {
    margin: 0.6rem;
  }

  &__delete-button {
    margin: 0.7rem 0 0.2rem;
    background-color: $button-color;
    color: $light-text-color;
    border: none;
    min-width: 7rem;
    height: 2rem;
    border-radius: 1rem;
    font-size: 0.8rem;
    padding: 0.4rem;
  }
}
</style>
