<template>
    <div class="jumbotron container">
        <h1 class="display-4 text-center">FastTyping</h1>
        <p class="lead text-center">Improve Your Typing Speed</p>
        <hr class="my-4">
        <div v-if="isFinish" class="alert alert-primary col-12">
            <div class="game-over">
                <h1 class="col-6">Game Over</h1>
                <div class="col-6 new-game">
                    <button @click="newGame" class="btn btn-primary btn-new-game">New Game</button>
                </div>
            </div>
            <p class="entered-words-number  col-6">Number of Words Entered per Minute: {{ numberWordsEnteredMinute }}</p>
            <span class=" col-6">Percentage Accuracy of Words: {{ percentageAccuracyWords}}% </span><br>
            <span class=" col-6"> Number of True: {{ trueCount }}</span><br>
            <span class=" col-6"> Number of False: {{ falseCount }}</span>
        </div>
        <div v-else>
            <div class="card">
                <div class="card-body">
                    <span v-for="(word,key) in words.filter((data,index) => index < 20)" :key="key" v-bind:class="key!=0 || writingWordControl " class="words ml-2">{{ word }}</span>
                </div>
            </div>
            <div class="card card-text">
                <div class="card-body bg-secondary">
                    <div class="input-group input-group-lg">
                        <input type="text" class="form-control" v-model="writingWord">
                        <div class="input-group-append">
                            <button class="btn btn-light" disabled type="button">{{ timer }} sec</button>
                            <button :disabled="isRunning" class="btn btn-light" type="button" @click="getWords">Refresh</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

// import wordListTurkish from '@/assets/turkish-words.json'
import wordListEnglish from '@/assets/english-words.json'

export default {
  data () {
    return {
      words: [],
      writingWord: null,
      isTrue: true,
      trueCount: 0,
      falseCount: 0,
      timer: 60,
      interval: false,
      isRunning: false,
      isFinish: false,
      // wordListTurkish: wordListTurkish,
      wordListEnglish: wordListEnglish
    }
  },
  mounted () {
    this.getWords()
  },
  watch: {
    writingWord (val) {
      if (!val || val === ' ') {
        this.writingWord = ''
        return
      }
      if (!this.isRunning) this.toggleTimer()
      const word = this.words[0].slice(0, val.length).toLowerCase()
      const userWorld = val.replace(' ', '').toLowerCase()
      this.isTrue = word === userWorld

      if (val.indexOf(' ') !== -1) {
        this.isTrue ? this.trueCount++ : this.falseCount++
        this.words.splice(0, 1)
        this.writingWord = ''
        this.isTrue = true
      }
    }
  },
  computed: {
    writingWordControl () {
      return this.isTrue ? 'focused-word' : 'focused-word focused-word-color bg-danger'
    },
    numberWordsEnteredMinute () {
      return 300 - this.words.length
    },
    percentageAccuracyWords () {
      const percentage = (100 / this.numberWordsEnteredMinute)
      const val = (percentage * this.trueCount)
      return isNaN(val) ? 0 : val.toFixed(2)
    }
  },
  methods: {
    newGame () {
      this.getWords()
      this.isFinish = false
      this.timer = 60
      this.isTrue = true
      this.isRunning = false
    },
    getWords () {
      this.words = this.wordListEnglish.sort(() => Math.random() - 0.5).splice(0, 300)
    },
    toggleTimer () {
      this.isRunning = true
      this.interval = setInterval(this.timeProcess, 1000)
    },
    timeProcess () {
      if (this.timer === 0) {
        clearInterval(this.interval)
        this.isFinish = true
        return
      }
      this.timer--
    }
  }
}
</script>

<style>
.game-over {
  display: flex;
}
.new-game {
  justify-content: end;
  display: flex;
}
.btn-new-game {
  height: fit-content;
}
.entered-words-number {
  font-size: 1.5rem;
}
.words {
  font-size: 24px;
  font-weight: 400;
}
.jumbotron .lead {
    margin-top: 20px;
}
.card-text {
    margin-top: 20px;
}
.focused-word {
  background-color: #dddddd;
  padding: 5px;
  border-radius: 5px;
}

.focused-word-color {
  color: #ffffff;
}
</style>
