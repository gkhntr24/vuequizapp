<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template v-slot:lead>
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group class="mb-2">
                <b-list-group-item
                        v-for="(answer,index) in answers" :key="index"
                        @click.prevent="selectAnswer(index)"
                        :class="answerClass(index)">
                    {{answer}}
                </b-list-group-item>
            </b-list-group>

            <b-button variant="primary" @click="submitAnswer" :disabled="selectedIndex==null || answered">Submit</b-button>
            <b-button variant="success" href="#" @click="next">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
    import _ from 'lodash';
    export default {
        name: "questionBox",
        props: {
            currentQuestion: Object,
            next: Function,
            increment:Function
        },
        data(){
          return {
              selectedIndex: null,
              shuffledAnswers: [],
              correctIndex: null,
              answered: false
          }
        },
        computed:{
            answers() {
                return this.shuffledAnswers
            }
        },
        watch:{
            currentQuestion: {
                immediate:true,
                handler(){
                    this.answered=false;
                    this.selectedIndex=null
                    this.shuffleAnswers()
                }
            }
        },
        methods:{
            selectAnswer(index){
                this.selectedIndex=index
            },
            shuffleAnswers(){
                let answers=[...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer];
                this.shuffledAnswers=_.shuffle(answers);
                this.correctIndex=this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
            },
            submitAnswer(){
                let isCorrect=false;
                if(this.selectedIndex===this.correctIndex)
                {
                    isCorrect=true;
                }
                this.increment(isCorrect)
                this.answered=true

            },
            answerClass(index){
                let answerClass='';
                if(!this.answered && this.selectedIndex===index)
                {
                    answerClass='selected';
                }
                else if(this.answered&&this.correctIndex===index)
                {
                    answerClass='correct';
                }
                else if(this.answered&&this.correctIndex!==index&&this.selectedIndex===index)
                {
                    answerClass='wrong';
                }
                else
                {
                    answerClass='';
                }

                return answerClass;
            }
        }
    }
</script>

<style scoped>
    .btn{
        margin: 0 5px;
    }

    .list-group-item
    {
        cursor: pointer;
    }

    .list-group-item:hover
    {
        background: #eeeeee;
    }

    .selected
    {
        background:lightblue;
    }

    .correct
    {
        background: lightgreen;
    }

    .wrong
    {
        background: lightcoral;
    }
</style>