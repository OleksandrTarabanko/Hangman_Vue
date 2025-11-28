<script>
import Hearts from './components/Hearts.vue';
import LettersToGuess from './components/LettersToGuess.vue';
import WrongLetters from './components/WrongLetters.vue';
import Keyboard from './components/Keyboard.vue';
import HangmanImage from './components/HangmanImage.vue';

export default {
  components: {
    Hearts,
    LettersToGuess,
    WrongLetters,
    Keyboard,
    HangmanImage,
  },

  data() {
    return {
      livesCount: 8,
      wordToGuess: null,
      wordToGuessLetters: null,
      wrongLetters: ["q", "w", "e"],
      currentState: [],
      keyboardWhiteList: "abcdefghijklmnopqrstuvwxyz",
    }
  },

  methods: {
    async startGame() {
      await this.resetValues()
    },

    async randomWord() {
      const request = "https://random-words-api.kushcreates.com/api?language=en&category=countries&type=lowercase&words=1";

      try {
        const response = await fetch(request);
        const data = await response.json();
        return data[0].word;
      } catch (error) {
        console.error("Promise was rejected with:", error);
      }
    },

    async resetValues() {
      this.livesCount = 8;
      this.wordToGuess = await randomWord();
      this.wordToGuessLetters = wordToGuess.split("");
      this.wrongLetters = [];
      // TODO: CHECK IF I CAN DELETE this.currentState = []
      this.currentState = [];
      this.currentState = this.wordToGuessLetters.map((letter) =>
        letter === " " ? " " : "___"
      );
    },

    handleKey(key) {
      if (this.livesCount > 0) {
        if (this.wordToGuessLetters.includes(key)) {
          for (const index in this.wordToGuessLetters) {
            if (this.wordToGuessLetters[index] === key) {
              this.currentState[index] = key
            }
          }
        }
      }
    }
  },

  async mounted() {
    this.wordToGuess = await this.randomWord()
    this.wordToGuessLetters = this.wordToGuess.split("")
    this.currentState = this.wordToGuessLetters.map(letter => letter === " " ? " " : "___")
  }
}

</script>

<template>
  {{ console.log(wordToGuessLetters) }}
  {{ console.log(currentState) }}

  <Hearts :hearts="livesCount" />
  <LettersToGuess :wordToGuessLetters="wordToGuessLetters" :currentState="currentState"></LettersToGuess>
  <WrongLetters :wrongLettersList="wrongLetters" />
  <Keyboard @handleKey(keyButton)="handleKey(key)" :keyboardWhiteList="keyboardWhiteList" />
  <HangmanImage :hearts="livesCount" />

</template>

<style scoped></style>
