<template>
  <div class="questionBox">
    <b-jumbotron>
      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item v-for="(answer, index) in shuffledAnswers" 
                          :key="index"
                          @click.prevent="selectedItem(index)"
                          :class="answerClass(index)">
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <div class="twoBtn">
        <b-button variant="primary"
                  @click="submitAnswer"
                  :disabled="selectedIndex === null || answered == true"
        >
          Submit
        </b-button>
        <b-button @click="nextQuestion" variant="success">Next</b-button>
      </div>
    </b-jumbotron>
  </div>
</template>


<script>
import _ from 'lodash'

export default {
  name: 'questionBox',
  props: {
  currentQuestion: Object,
  nextQuestion: Function,
  increment: Function
},

data() {
  return {
    selectedIndex: null,
    correctIndex: null,
    shuffledAnswers: [],
    answered: false
  }
},

computed: {
  answers() {
    let answer = [...this.currentQuestion.incorrect_answers];
    answer.push(this.currentQuestion.correct_answer);
    return answer;
  }
},

watch: {
  currentQuestion: {
    immediate: true,
    handler() {
      this.selectedIndex = null
      this.answered = false
      this.shuffleAnswers()
    }
  }
},

methods: {
  selectedItem(index) {
    this.selectedIndex = index;
  },
  submitAnswer() {
    let isCorrect = false

    if(this.selectedIndex === this.correctIndex) {
      isCorrect = true
    }
    this.answered = true

    this.increment(isCorrect)
  },
  shuffleAnswers() {
    let answer = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
    this.shuffledAnswers = _.shuffle(answer)
    this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
  },
  answerClass(index) {
    let answerClass = ''
    if(!this.answered && this.selectedIndex === index) {
      answerClass = 'selected'
    }
    else if(this.answered && this.correctIndex === index) {
      answerClass = 'correct'
    }
    else if(this.answered && this.selectedIndex === index) {
      answerClass = 'incorrect'
    }
    return answerClass;
  }
}
}

</script>


<style scoped>
.questionBox {
  text-align: center;
}
.twoBtn {
  margin-top: 15px;
}
.btn {
  margin-right: 5px;
}
.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}
.selected {
  background: rgb(145, 145, 219);
}
.correct {
  background: rgb(94, 219, 94);
}
.incorrect {
  background: rgb(235, 34, 34);
}
</style>