<template>
  <div>
    <b-jumbotron>
      <template v-slot:lead>{{currentQuestion.question}}</template>

      <hr class="my-4" />

      <b-list-group>
        <!-- Answer is gotten from the shuffledAnswers array -->
        <b-list-group-item
          v-for="(answer,index) in shuffledAnswers"
          :key="index"
          @click="selectIndex(index)"
          :class="classComputed(index)"
        >{{answer}}</b-list-group-item>
      </b-list-group>
      <!-- v-for="answer in answers :key='answer" can also be used since each answer is unique-->

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex===null || answered"
      >Submit</b-button>
      <b-button
        variant="success"
        href="#"
        @click="next"
        :disabled="!answered || indexOfCurrentQuestion===14"
      >Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
    indexOfCurrentQuestion: Number
  },
  data() {
    return {
      //   This is used to capture the index clicked by the user
      selectedIndex: null,
      shuffledAnswers: [],
      correctIndex: null,
      answered: false
    };
  },
  computed: {
    //   Computes an array containing the incorrect and correct answers
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    }
  },
  methods: {
    //   Set the index clicked to the selected index
    selectIndex(index) {
      this.selectedIndex = index;
    },
    // Shuffle the computed propery 'answers' using lodash to get the shuffled array
    shuffleArray() {
      this.shuffledAnswers = _.shuffle([...this.answers]);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    submitAnswer() {
      let isCorrect = false;

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.increment(isCorrect);
      this.answered = true;
    },
    classComputed(index) {
      let classComputed = "";

      if (this.selectedIndex === index && !this.answered) {
        classComputed = "selected";
      } else if (this.correctIndex === index && this.answered) {
        classComputed = "correct";
      } else if (
        this.selectedIndex === index &&
        this.correctIndex !== index &&
        this.answered
      ) {
        classComputed = "incorrect";
      }
      return classComputed;
    }
  },
  //   Check for changes in current question and set selectedindex to null and shuffle the answers
  watch: {
    currentQuestion: {
      // To make it run at the start when it is mounted without necessarily defining mounted hook
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffleArray();
        this.answered = false;
      }
    }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 1rem;
}

.list-group-item:hover {
  outline: 0.2rem solid lightblue;
  cursor: pointer;
}

.btn {
  margin: 0 1rem;
}

.selected {
  background-color: yellow;
}
.correct {
  background-color: lightgreen;
  transform: scale(1.2);
}
.incorrect {
  background-color: red;
}
</style>