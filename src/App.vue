<template>
  <header>
    <h1>{{ mainHeaderText }}</h1>
    <h2>{{ secondHeaderText }}</h2>
    <form>
      <p>{{ languageText }}</p>
      <div>
        <label>
          polski
          <input
            type="radio"
            name="language"
            value="polish"
            v-model="chosenLanguage"
          />
        </label>
        <label>
          english
          <input
            type="radio"
            name="language"
            value="english"
            v-model="chosenLanguage"
          />
        </label>
      </div>
    </form>
  </header>
  <Input @updateList="forceRerender" :chosenLanguage="chosenLanguage" />
  <TimerList :key="componentKey" :chosenLanguage="chosenLanguage" />
</template>

<script>
import Input from './components/Input.vue';
import TimerList from './components/TimerList.vue';

export default {
  name: 'App',
  data() {
    return {
      componentKey: 0,
      chosenLanguage: 'polish',
    };
  },
  computed: {
    mainHeaderText() {
      if (this.chosenLanguage === 'polish') {
        return 'Licznik czasu';
      } else {
        return 'Timer';
      }
    },
    secondHeaderText() {
      if (this.chosenLanguage === 'polish') {
        return 'Jak dużo czasu zostało do wybranej daty?';
      } else {
        return 'How much time has left to the chosen date?';
      }
    },
    languageText() {
      if (this.chosenLanguage === 'polish') {
        return 'Wybierz język';
      } else {
        return 'Choose language';
      }
    },
  },
  components: {
    Input,
    TimerList,
  },
  methods: {
    forceRerender() {
      this.componentKey++;
    },
  },
};
</script>

<style lang="scss">
@import './colors';

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  background: $background-color;
  max-width: 40rem;
  margin-left: auto;
  margin-right: auto;
}
</style>
