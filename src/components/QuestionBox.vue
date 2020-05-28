<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{ currentQuestion.question }}</template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >
        {{ answer }}
        </b-list-group-item>
      </b-list-group>
      
      <b-button variant="primary" href="#"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
      Submit
      </b-button>
      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash';

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increament: Function,
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
    };
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
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
    },
    submitAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.increament(isCorrect);
    },
    answerClass(index) {
      let answerClass = '';
      !this.answered && this.selectedIndex === index ?
      answerClass = 'selected' :
      this.answered && this.correctIndex === index ?
      answerClass = 'correct' :
      this.answered && this.correctIndex !== index && this.selectedIndex === index ?
      answerClass = 'incorrect' : ''
      return answerClass;
    }
  }
};
</script>

<style scoped>
  .list-group {
    margin-bottom: 50px;
  }

  .list-group-item:hover {
    background: #EEE;
    cursor: pointer;
  }

  .btn {
    margin: 0 5px;
  }

  .selected {
    background-color: lightblue;
  }

  .correct {
    background-color: lightgreen;
  }

  .incorrect {
    background-color: red;
  }
</style>