<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
    ></Header>

    <b-container :class="[numTotal === 10 ? 'displayHide' : 'displayShow']" class="bv-example-row">
      <b-row>
        <b-col></b-col>
        <b-col sm="6" offset=""> 
          <QuestonBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :nextQuestion="next"
            :increment="increment">
           </QuestonBox> 
        </b-col>
        <b-col></b-col>
      </b-row>
    </b-container>

  </div>
</template>


<script>
import Header from './components/Header'
import QuestonBox from './components/QuestionBox'

export default {
  name: 'app',
  components: {
    Header, 
    QuestonBox
  },

  data() {
    return {
      questions: [],
      index : 0,
      numCorrect: 0,
      numTotal: 0
    }
  },

  methods: {
    next() {
      this.index++
    },
    increment(isCorrect) {
      if(isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    }
  },

  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=18&difficulty=easy&type=multiple', {
      method: 'get'
    })
    .then((res) => {
      return res.json()
    })
    .then((jsonData) => {
      this.questions = jsonData.results
    })
  }
}
</script>


<style>
  .displayShow {
    display: block;
  }
  .displayHide {
    display: none;
  }
</style>
