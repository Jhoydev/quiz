<template>
    <div class="question-box-container">
         <b-jumbotron>
            <h4 v-html="currentQuestion.question"></h4>
            <hr class="my-4">
            <b-list-group>
                <b-list-group-item 
                    v-for="(answer,index) in answers" 
                    :key="index"
                    @click.prevent="selectAnswer(index)"
                    :class="classSubmitAnswer(index)"
                    v-html="answer"
                >{{ answer }}</b-list-group-item>
            </b-list-group>
            <b-button :disabled="isDisabledSubmit" @click="submitAnswer" variant="primary" href="#">Submit</b-button>
        </b-jumbotron>
    </div>
</template>
<script>
import _ from 'lodash'
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function,
        statusQuiz: String
    },
    data() {
        return {
            selectedIndex: null,
            shuffledAnswers: [],
            answerCorrectIndex: null,
            answered: false,
            numQuestion : 1
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        },
        isDisabledSubmit(){
            return this.selectedIndex == null
        }
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null
                this.answerCorrectIndex = null
                this.shuffleAnswers()
                this.answered = false
            }
        }
    },
    methods: {
        selectAnswer(index){
            this.selectedIndex = index
            this.answered = false            
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.answerCorrectIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer(){
            this.increment(this.answerCorrectIndex === this.selectedIndex)
            this.answered = true
            
        },
        classSubmitAnswer(index){
            let classAnswer = ''
            if (this.selectedIndex === index){
                classAnswer = 'selected'
            }
            if (this.selectedIndex === index && this.answerCorrectIndex  === index && this.answered){
                classAnswer = 'correct'
            }

            if (this.selectedIndex === index && this.answerCorrectIndex  !== index && this.answered){
                classAnswer = 'incorrect'
            }
            return classAnswer;
        }
    },
}
</script>
<style scoped>
    .list-group{
        margin-bottom: 15px;
    } 
    .list-group-item:hover{
        background-color: #EEE;
        cursor: pointer;
    } 
    .selected{
        background-color: lightblue;
    }
    .correct{
        background-color: lightgreen;
    }
    .incorrect{
        background-color: red;
        color:white;
    }
    .btn {
        margin: 0 5px;
    }   
</style>