<template>
  <header class="">
    <b-nav tabs align="center">
      <b-nav-item disabled><strong>Vue Quiz App</strong></b-nav-item>
      <b-nav-item disabled>
        Correct Answers: {{numCorrect}}/{{numTotal}}
        </b-nav-item>
    </b-nav>
    <b-progress
      :value="progress * 10"
      :variant="progress === 10 ? 'success' : ''"
    ></b-progress>
  </header>
</template>

<script>
export default {
  props: [
    'progress',
    'numCorrect',
    'numTotal',
  ]
}
</script>

<style scoped>
  header {
    margin-top: 20px;
  }
  .progress { 
    height: 0.1rem;
    margin-bottom: 30px;
    border-radius: 0;
  }
</style>
