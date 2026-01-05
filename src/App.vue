<script>
import Hearts from './components/Hearts.vue';
import LettersToGuess from './components/LettersToGuess.vue';
import WrongLetters from './components/WrongLetters.vue';
import Keyboard from './components/Keyboard.vue';
import HangmanImage from './components/HangmanImage.vue';
import Modal from './components/Modal.vue';

export default {
  components: {
    Hearts,
    LettersToGuess,
    WrongLetters,
    Keyboard,
    HangmanImage,
    Modal,
  },

  data() {
    return {
      livesCount: 8,
      wordToGuess: null,
      wordToGuessLetters: null,
      wrongLetters: [],
      currentState: [],
      keyboardWhiteList: "abcdefghijklmnopqrstuvwxyz'-",
      guessed: false
    }
  },

  methods: {
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

    handleButtonKey(key) {
      const letter = key.keyPressed.toLowerCase()
      if (this.livesCount > 0) {
        if (this.wordToGuessLetters.includes(letter)) {
          for (const index in this.wordToGuessLetters) {
            if (this.wordToGuessLetters[index] === letter) {
              this.currentState[index] = letter.toUpperCase()
            }
          }

          if (!this.currentState.includes("___")) {
            this.guessed = true
            document.onkeydown = null
          }

          return;
        }
        if (!this.wrongLetters.includes(letter)) {
          this.wrongLetters.push(letter)
          this.livesCount -= 1
        }
      } else {
        document.onkeydown = null
      }
    },

    handleKeyboardKey(key) {
      const letter = key
      if (this.livesCount > 0) {
        if (this.wordToGuessLetters.includes(letter)) {
          for (const index in this.wordToGuessLetters) {
            if (this.wordToGuessLetters[index] === letter) {
              this.currentState[index] = letter.toUpperCase()
            }
          }

          if (!this.currentState.includes("___")) {
            this.guessed = true
            document.onkeydown = null
          }

          return;
        }
        if (!this.wrongLetters.includes(letter)) {
          this.wrongLetters.push(letter)
          this.livesCount -= 1
        }
      } else {
        document.onkeydown = null
      }
    },

    async restart() {
      this.livesCount = 8;
      this.wordToGuess = await this.randomWord();
      this.wordToGuessLetters = this.wordToGuess.split("");
      this.wrongLetters = [];
      this.currentState = this.wordToGuessLetters.map((letter) =>
        letter === " " ? " " : "___"
      );
      this.guessed = false
      document.onkeydown = (e) => {
        if (this.keyboardWhiteList.includes(e.key)) {
          this.handleKeyboardKey(e.key)
        }
      }
    },
  },

  async mounted() {
    this.wordToGuess = await this.randomWord()
    this.wordToGuessLetters = this.wordToGuess.split("")
    this.currentState = this.wordToGuessLetters.map(letter => letter === " " ? " " : "___")
    document.onkeydown = (e) => {
      if (this.keyboardWhiteList.includes(e.key)) {
        this.handleKeyboardKey(e.key)
      }
    }
  }
}

</script>

<template>

  <Hearts :hearts="livesCount" />
  <LettersToGuess :currentState="currentState" />
  <WrongLetters :wrongLettersList="wrongLetters" />
  <Keyboard @handleKey="handleButtonKey" :keyboardWhiteList="keyboardWhiteList" />
  <HangmanImage :hearts="livesCount" />
  <Modal :guessed="guessed" :hearts="livesCount" :wordToGuess="wordToGuess" @restart="restart" />

</template>

<style scoped></style>
