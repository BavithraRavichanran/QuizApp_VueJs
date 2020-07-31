<template>
<div class="question-box-container">
  <b-jumbotron >
    <template v-slot:lead >
      {{currentQuestion.question}}
    </template>

    <hr class="my-4">

    <p class=".list-group"> 
          <b-list-group> 
        <b-list-group-item  v-for="(answer,index) in shuffledAnswers" :key="index"
        @click="selectAnswer(index)"
        :class="[ 
        !answered && selectedIndex === index ? 'selected':
        answered && selectedIndex === index && correctIndex === index? 'correct': 
        answered && selectedIndex === index && correctIndex !== index? 'incorrect': '']"
        >{{answer}} </b-list-group-item>
        </b-list-group>
    </p>

    <b-button variant="primary" class="btn"
    @click="submitAnswer"
    :disabled = "selectedIndex===null || answered"
    >Submit</b-button>
    <b-button variant="success" href="#" class="btn" @click="next">Next</b-button>
  </b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash'
export default {
    props :{
        currentQuestion: Object,
        next: Function,
        increment:Function 
    },
    data:function() {
        return{
            selectedIndex:null,
            shuffledAnswers: [],
            correctIndex : null,
            answered: false
        }
    },
    watch:{
        // currentQuestion(){
        //     this.selectedIndex = null;
        //     this.shuffleAnswers()
        // }

        //inorder to shuffle the first element we have to make the currentquest to obj
        //so that watch is done when the props is get

        currentQuestion: {
            immediate: true,
            handler(){
                this.selectedIndex = null;
                this.answered =false;
                this.shuffleAnswers()
            }
        }
    },
    //argument will pass
    methods: {
        selectAnswer(index){
            this.selectedIndex =index;
        },
        shuffleAnswers(){
            console.log("shuffle")
            let answers= [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
            this.shuffledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            
        },
        submitAnswer(){
            let isCorrect = false
            if(this.selectedIndex === this.correctIndex){
                    isCorrect = true;
            }
            this.answered = true;
            this.increment(isCorrect);
        }
    },
    computed: {
        answers(){
        let answers= [...this.currentQuestion.incorrect_answers];
        answers.push(this.currentQuestion.correct_answer)
        return answers;
        }
    },
    
}
</script>

<style scoped>
.list-group{
    margin-bottom: 15px;
}

.list-group-item:hover{
   cursor: pointer;
   background: #EEE;
}
.btn{
    margin: 0 5px;
}
.selected{
    background-color: rgb(139, 183, 235);
}
.correct{
    background-color: lightgreen;
}
.incorrect{
    background-color: rgb(230, 94, 94);
}
</style>
