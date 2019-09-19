<template>
  <div id="app">
    <Header :numTotal="numTotal" :numCorrect="numCorrect" @reload="reload" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <!-- QuestionBox component is only shown when it is populated -->
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="nextQuestion"
            :increment="increment"
            :indexOfCurrentQuestion="index"
          />
          <div v-else>
            <h1>Loading...</h1>
          </div>
        </b-col>
      </b-row>
    </b-container>
    <Modal :numCorrect="numCorrect" :numTotal="numTotal" v-if="numTotal === 15" @reload="reload" />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
import Modal from "./components/Modal.vue";

export default {
  name: "app",
  components: {
    Header,
    QuestionBox,
    Modal
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    };
  },
  methods: {
    // Increase the index by one if index is in range
    nextQuestion() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    },
    reload() {
      window.location.reload();
    }
  },
  // When mounted, fetch the quiz api
  mounted() {
    fetch("https://opentdb.com/api.php?amount=15&category=21&type=multiple", {
      method: "GET"
    })
      .then(response => response.json())
      .then(myJson => (this.questions = myJson.results));
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
