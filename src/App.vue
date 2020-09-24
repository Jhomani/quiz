<template>
  <div id="app">
    <Header 
      :indexQuestion="index"
      :corrects="numberCorrects"
    /> 

    <b-container>
      <b-row>
        <b-col sm="8" offset-sm="2"  >
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="handleNext"
            :corrects= "corrects"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'App',
  data(){
    return{
      questions:[],
      index:0,
      numberCorrects:0,
    }
  },
  components: {
    Header,
    QuestionBox,
  },
  created(){
    this.handleApi();
  },
  methods:{
    async handleApi(){
      try{
        const result = await fetch('https://opentdb.com/api.php?amount=10&category=19&difficulty=medium&type=multiple', {
          method:'get'
        })
        const data = await result.json()
        
        this.questions = data.results;
      }catch(err){
        console.log(err)
      }
    },
    handleNext(Index){
      if(Index === null){
        this.$bvModal.show("modal-next")
        return;
      }      

      if(this.index === 9)return;
      this.index++;
    }, 
    corrects(){
      this.numberCorrects++;
    }, 
  }
}
</script>

<style>
</style>
