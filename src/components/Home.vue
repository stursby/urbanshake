<template>
  <div @click="handleShake">
    Home
    <div v-if="word">
      <p>{{ word.word }}</p>
      <p>{{ word.definition }}</p>
      <p>{{ word.example }}</p>
      <pre>
        {{ word }}
      </pre>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Shake from 'shake.js'
const API = 'https://api.urbandictionary.com/v0/random'

export default {
  name: 'Home',

  data () {
    return {
      words: [],
      word: null
    }
  },

  mounted () {
    this.initShake()
    this.fetchWords()
  },

  methods: {
    fetchWords: function () {
      if (this.words.length) return
      return axios.get(API).then((res) => {
        const { list } = res.data
        this.words = list
      })
    },

    initShake: function () {
      const shakeEvent = new Shake()
      shakeEvent.start()
      window.addEventListener('shake', this.handleShake, false)
    },

    handleShake: function () {
      if (this.words.length) {
        this.word = this.words[0]
        this.words.shift()
      } else {
        this.fetchWords().then(() => {
          this.handleShake()
        })
      }
    }
  }
}
</script>
