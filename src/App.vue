<template>
  <div id="app">
    <b-container class="bv-example-row">
      <b-row align-h="center">
        <b-col sm="12">
          <Header
            :correctQuestions="correctQuestions"
            :totalQuestions="totalQuestions"
            :countQuestion="countQuestion"
            :statusQuiz="statusQuiz"
          ></Header>
          <br>
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
            :statusQuiz="statusQuiz"
          ></QuestionBox>
          <ResultQuiz
            v-if="!questions.length && statusQuiz == 'finished'"
            :listResult="listResult"
          ></ResultQuiz>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import ResultQuiz from './components/ResultQuiz.vue'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox,
    ResultQuiz
  },
  data() {
    return {
      questions: [],
      index: 0,
      correctQuestions: 0,
      totalQuestions: 0,
      countQuestion: 1,
      statusQuiz: 'active',
      listResult: []
    }
  },
  methods: {
    next(){
      this.index++
      this.countQuestion++
      if ((this.totalQuestions-1) == this.index){
        this.statusQuiz = 'last'
      }
    },
    increment(isCorrect){
      this.statusQuiz = 'active'
      if (isCorrect){
        this.listResult.push(this.questions[this.index])
        this.correctQuestions++
      }

      if (this.totalQuestions == this.countQuestion){
        this.questions = []
        this.statusQuiz = 'finished'
      }
      
      if (this.totalQuestions > this.countQuestion){
        this.next()
      }

    }
  },
  mounted() {
    fetch('https://opentdb.com/api.php?amount=10&category=9&type=multiple',{
      method: 'get'
    }).then((response)=> response.json())
      .then((jsonData)=>{
        this.questions = jsonData.results
        this.totalQuestions = this.questions.length
      })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
