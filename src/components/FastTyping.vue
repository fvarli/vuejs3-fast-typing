<template>
  <div class="jumbotron container">
    <h1 class="display-4 text-center">FastTyping</h1>
    <p class="lead text-center">Improve your Typing Speed</p>
    <div>
      Number of True: {{ trueCount }}
      Number of False: {{ falseCount }}
    </div>
    <hr class="my-4">
    <div class="card">
      <div class="card-body">
        <span v-for="(word,key) in words" :key="key" v-bind:class="key!=0 || writingWordControl " class="words ml-2">{{ word }}</span>
      </div>
    </div>
    <div class="card">
      <div class="card-body bg-secondary">
        <div class="input-group input-group-lg">
          <input type="text" class="form-control" v-model="writingWord">
          <div class="input-group-append">
            <button class="btn btn-light" disabled type="button">1:00</button>
            <button class="btn btn-light" type="button">Refresh</button>
          </div>
        </div>
      </div>
    </div>
    {{ writingWord }}
  </div>
</template>

<script>
export default {
  data () {
    return {
      words: ['test', 'fred'],
      writingWord: null,
      isTrue: true,
      trueCount: 0,
      falseCount: 0
    }
  },
  watch: {
    writingWord (val) {
      const word = this.words[0].slice(0, val.length)
      const userWorld = val.replace(' ', '')
      this.isTrue = word === userWorld

      if (val.indexOf(' ') !== -1) {
        this.isTrue ? this.trueCount++ : this.falseCount++
        this.words.splice(0, 1)
        this.writingWord = ''
      }
      // console.log(word)
      /* if (val === word) {
        console.log('true')
        this.isTrue = true
      } else {
        console.log('false')
        this.isTrue = false
      } */
    }
  },
  computed: {
    writingWordControl () {
      return this.isTrue ? 'focused-word' : 'focused-word focused-word-color bg-danger'
    }
  }
}
</script>

<style>
  .words {
    font-size: 24px;
    font-weight: 400;
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
