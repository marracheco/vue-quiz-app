<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
      :progress="index"
    />
    <b-container>
      <b-row>
        <b-col md="6" offset-md="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :index="index + 1"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
  import Header from './components/Header.vue'
  import QuestionBox from './components/QuestionBox.vue'

  export default {
    name: 'App',
    components: {
      Header,
      QuestionBox,
    },
    data() {
      return {
        questions: [],
        index: 0,
        numCorrect: 0,
        numTotal: 0,
      }
    },
    methods: {
      next() {
        this.index++
      },
      increment(isCorrect) {
        if (isCorrect) {
          this.numCorrect++;
        }
        this.numTotal++;
      }
    },
    mounted: function() {
      fetch('https://opentdb.com/api.php?amount=10&category=27&difficulty=easy&type=multiple')
        .then(resp => resp.json())
        .then(jsonData => {
          this.questions = jsonData.results;
        });
    }
  }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
  }
</style>
