<template>
  <div id="app">
    <Header :numTotal="numTotal" :numCorrect="numCorrect"/>
    <b-container class="bv-example-row">
      <b-row class="justify-content-center">
        <b-col sm="6">
          <QuestionBox class="mt-5" v-if="questions.length" :currentQuestion="questions[index]" :next="next" :increment="increment" />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/header.vue'
import QuestionBox from './components/questionBox'

export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  data(){
    return {
      'questions': [],
      'index': 0,
      'numCorrect': 0,
      'numTotal': 0
    }
  },
  methods: {
    next(){
      this.index++;
    },
    increment(isCorrect){
      console.log(isCorrect);
      if(isCorrect){
        this.numCorrect++;
      }
      this.numTotal++;
    }
  },
  mounted() {
    fetch('https://opentdb.com/api.php?amount=10&category=27&type=multiple',{
      method:'get'
    }).then((response)=>{
      return response.json()
    }).then((jsonData)=>{
      this.questions = jsonData.results
      console.log(this.questions)
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
