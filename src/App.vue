<template>
  <div id="app">
    <h1>Quiz</h1>
    <img v-if="cat_url" :src="cat_url" width="500" alt="" @click="cat_url = ''">
    <div v-else v-for="(frage, index) of fragen" :key="index">
      {{frage.frage}}
      <ul>
        <li v-for="(antwort, index) of frage.antworten" :key="index" @click="check(antwort)">{{antwort.antwort}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'app',
  components: {
  },
  data() {
    return {
      cat_url: '',
      show_cat: false,
      fragen: [
        {
          frage: 'Wie lange dauert ein Fussballspiel?',
          antworten: [
            {antwort: '60', correct: false},
            {antwort: '90', correct: true},
            {antwort: '120', correct: false},
            {antwort: 'Beliebig', correct: false},
          ]
        }
      ]
    }
  },
  methods: {
    async check(antwort) {
      if (antwort.correct) {
        let response = await axios.get('https://api.thecatapi.com/v1/images/search?mime_types=gif&format=json')
        this.cat_url = response.data[0].url
        alert('Super!')
      } else {
        alert('Falsch!')
      }
    }
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
