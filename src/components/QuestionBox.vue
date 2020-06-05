<template>
  <div>
    <b-jumbotron>
      <template v-slot:lead>
        {{`${index}. ${currentQuestion.question}`}}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          href="#"
          v-for="(answer, i) in answers" :key="i"
          @click="selectAnswer(i)"
          :class="[selectedIndex === i ? 'active' : '']"
        >
          {{answer}}
        </b-list-group-item>
      </b-list-group>

      <hr class="my-4">

      <b-button variant="primary" href="#">Submit</b-button>
      &nbsp;
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash';

export default {
  props: {
    currentQuestion: Object,
    index: Number,
    next: Function,
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnsers: [],
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
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
    }
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
