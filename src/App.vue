<template>
  <div id="app">
    <app-header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    ></app-header>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <app-question-box
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increament="increament"
          ></app-question-box>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import QuestionBox from "./components/QuestionBox.vue";
import Header from "./components/Header.vue";

export default {
  name: "App",
  components: {
    "app-header": Header,
    "app-question-box": QuestionBox
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
      this.index++;
    },
    increament(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    }
  },
  mounted() {
    fetch('https://opentdb.com/api.php?amount=10&category=31&type=multiple', {
      method: 'GET'
    })
      .then(response => {
        return response.json();
      })
      .then(jsonData => {
        this.questions = jsonData.results;
      });
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
