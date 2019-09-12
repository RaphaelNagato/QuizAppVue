<template>
  <div>
    <b-jumbotron>
      <template v-slot:lead>{{currentQuestion.question}}</template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer,index) in answers"
          :key="index"
          @click="selectIndex(index)"
        >{{answer}}</b-list-group-item>
      </b-list-group>
      <!-- v-for="answer in answers :key='answer" can also be used since each answer is unique-->

      <b-button variant="primary" href="#">Submit</b-button>
      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentQuestion: Object,
    next: Function
  },
  data() {
      return {
          selectedIndex: null
      }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  methods: {
      selectIndex(index) {
          this.selectedIndex = index;
      }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 1rem;
}

.btn {
  margin: 0 1rem;
}
</style>