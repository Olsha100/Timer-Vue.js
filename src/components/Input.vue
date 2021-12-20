<template>
  <form class="input" @submit.prevent="handleSubmit">
    <label for="newTimer">{{ inputLabelText }}</label>
    <input
      type="datetime-local"
      class="input__date"
      name="newTimer"
      id="newTimer"
      v-model="newTimer"
    />
    <button type="submit" class="input__submit-button">{{ buttonText }}</button>

    <div v-if="errorSimilarTimer" class="input__error">
      {{ errorSimilarTimerText }}
    </div>
    <div v-if="errorDateInPast" class="input__error">
      {{ errorDateInPastText }}
    </div>
  </form>
</template>

<script>
export default {
  name: 'Input',
  props: ['chosenLanguage'],
  data() {
    return {
      currentDate: new Date(),
      newTimer: '',
      timers: [],
      errorSimilarTimer: false,
      errorDateInPast: false,
    };
  },
  computed: {
    tomorrowDate() {
      return `${this.currentDate.getFullYear()}-${
        this.currentDate.getMonth() + 1
      }-${this.currentDate.getDate() + 1}`;
    },
    inputLabelText() {
      if (this.chosenLanguage === 'polish') {
        return 'Wskaż datę i godzinę licznika';
      } else {
        return 'Choose date and time of the timer';
      }
    },
    buttonText() {
      if (this.chosenLanguage === 'polish') {
        return 'Utwórz licznik';
      } else {
        return 'Create a timer';
      }
    },
    errorSimilarTimerText() {
      if (this.chosenLanguage === 'polish') {
        return 'Licznik z wybraną datą już istnieje';
      } else {
        return 'Timer with similar date already exists';
      }
    },
    errorDateInPastText() {
      if (this.chosenLanguage === 'polish') {
        return 'Wybrana data jest z przeszłości';
      } else {
        return 'Chosen date is from the past';
      }
    },
  },
  methods: {
    handleSubmit() {
      this.errorSimilarTimer = false;
      this.errorDateInPast = false;

      if (this.newTimer) {
        //Getting data saved in local storage
        if (localStorage.getItem('timers')) {
          this.timers = JSON.parse(localStorage.getItem('timers'));
        }
        //Error handling
        if (this.timers.includes(this.newTimer)) {
          this.errorSimilarTimer = true;
        }

        if (new Date(this.newTimer) < this.currentDate) {
          this.errorDateInPast = true;
        }

        //Passing data to local storage
        if (
          new Date(this.newTimer) > this.currentDate &&
          !this.timers.includes(this.newTimer)
        ) {
          this.timers.push(this.newTimer);
          localStorage.setItem('timers', JSON.stringify(this.timers));

          //Custom event needed to rerender TimeList component
          this.$emit('updateList');
        }
      }

      //Clearing newTimer which is also the input value
      this.newTimer = '';
    },
  },
};
</script>

<style lang="scss">
@import '../colors';

.input {
  margin-top: 1rem;

  label {
    display: block;
  }

  &__date {
    border: none;
    border-bottom: 1px solid $dark-text-color;
    width: 10rem;
    margin: 0 0.5rem;
  }

  &__submit-button {
    margin: 0.5rem;
    background-color: $button-color;
    color: $light-text-color;
    border: none;
    width: 9rem;
    height: 2rem;
    border-radius: 1rem;
    font-size: 0.9rem;
  }

  &__error {
    font-size: 0.7rem;
    color: $error-text-color;
  }
}
</style>
