<template>
  <div id="app">
    <h1>Wähle dein Quiz</h1>

    <ul>
      <li v-for="quiz of quizzes" :key="quiz.name" @click="startQuiz(quiz.id)">{{quiz.name}}</li>
    </ul>

    <!-- <img v-if="cat_url" :src="cat_url" width="500" alt="" @click="cat_url = ''">
    <div v-else v-for="(frage, index) of fragen" :key="index">
      {{frage.frage}}
      <ul>
        <li v-for="(antwort, index) of frage.antworten" :key="index" @click="check(antwort)">{{antwort.antwort}}</li>
      </ul>
    </div> -->
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
        },
      ],
      quizzes: []
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
    },
    async startQuiz(quizId) {
      console.log("Starte das Quiz mit ID", quizId)
      let response = await axios.get(`/api/quiz/${quizId}/questions`)
      console.log(response.data)
    }
  },
  computed: {
    anzahl_fragen_string() {
      let anzahl_fragen = this.fragen.length
      if (anzahl_fragen === 0) {
        return 'ohne Fragen'
      } else if (anzahl_fragen === 1) {
        return 'mit einer Frage'
      } else {
        return `mit ${anzahl_fragen} Fragen`
      }
      return this.fragen.length
    }
  },
  async created() {
    let response = await axios.get('/api/quiz')
    console.log("Daten vom API", response.data)
    this.quizzes = response.data
  }
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
