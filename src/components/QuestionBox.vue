<template>
  <div>
    <b-jumbotron>
      <div class="logo">
        <img src="../assets/logo.png" alt="Vue Quiz" width="60" />
      </div>

      <p class="lead">
        {{`${index}. ${currentQuestion.question}`}}
      </p>

      <b-list-group>
        <b-list-group-item
          href="#"
          v-for="(answer, i) in answers" :key="i"
          @click="!answered && selectAnswer(i)"
          :variant="answerVariant(i)"
        >
          {{answer}}
        </b-list-group-item>
      </b-list-group>

      <div class="btn-container">
        <b-button
          variant="success"
          @click="submitAnswer"
          v-if="!(selectedIndex === null || answered)"
        >
          Submit
        </b-button>
        <b-button
          v-if="answered && index < 10"
          variant="info"
          @click="next"
        >
          Next
          <BIconArrowRight />
        </b-button>
      </div>

    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash';
import { BIconArrowRight } from 'bootstrap-vue'

export default {
  components: {
    BIconArrowRight
  },
  props: {
    currentQuestion: Object,
    index: Number,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnsers: [],
      correctIndex: null,
      answered: false,
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      }
    }
  },
  methods: {
    selectAnswer(i) {
      this.selectedIndex = i;
    },
    shuffleAnswers() {
      let answers = [
        this.currentQuestion.correct_answer,
        ...this.currentQuestion.incorrect_answers,
      ];
      this.shuffledAnsers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnsers.indexOf(this.currentQuestion.correct_answer);
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    answerVariant(i) {
      let variant = 'default';
      if (!this.answered && this.selectedIndex === i) {
        variant = 'primary';
      } else if (this.answered && this.correctIndex === i) {
        variant = 'success';
      } else if (this.answered && this.selectedIndex === i && this.correctIndex !== i) {
        variant = 'danger';
      }
      return variant;
    },
  },
  computed: {
    answers() {
      if (!this.currentQuestion) return;
      return [
        this.currentQuestion.correct_answer,
        ...this.currentQuestion.incorrect_answers,
      ]
    }
  },
}
</script>

<style scoped>
  .jumbotron {
    padding-top: 20px;
  }
  .logo {
    text-align: center;
    margin-bottom: 10px;
  }
  .lead {
    text-align: center;
  }
  .list-group-item {
    text-align: left;
  }
  .btn-container {
    text-align: center;
    margin-top: 30px;
  }
  .btn-container .btn + .btn {
    margin-left: 10px;
  }
</style>