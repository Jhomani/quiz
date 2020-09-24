<template>
  <div class="container-question-box">
     <b-jumbotron class="m-auto text-center">
      <p>{{ currentQuestion.question }}</p>

      <hr class="my-4">

      <b-list-group class="answers" >
        <b-list-group-item 
          v-for="(item,index) in answers" :key="index"
          @click.stop="selectAnswer(index)"
          :class="handleAnswerClass(index)" 
          :disabled="correctIndex !==null"
        >
          {{item}}
        </b-list-group-item>
      </b-list-group>

      <b-modal id="modal-next" size="sm" centered hide-footer>
        <p class="text-center">
          Submit datas
        </p>
      </b-modal>

      <div class="mt-4 d-flex justify-content-around">
        <b-button variant="info" @click="submit" :disabled="selectIndex === null || correctIndex !== null">Submit</b-button>
        <b-button variant="primary" @click="next(correctIndex)">Next</b-button>
      </div>
    </b-jumbotron> 
  </div>
</template>
<script>
  export default{
    props:{
      currentQuestion: Object,
      next: Function,
      corrects: Function,
    },
    data(){
      return{
        selectIndex:null,
        error:null,
        correctIndex:null,
      }
    },
    computed:{
      answers(){
        const indexCA = Math.floor(Math.random()*4);
        
        const totalAnswers = [...this.currentQuestion.incorrect_answers];

        totalAnswers.splice(indexCA, 0, this.currentQuestion.correct_answer);
      
        return totalAnswers
      }
    },
    watch:{
      currentQuestion: {
        immediate:true,
        handler(){
          this.selectIndex = null;
          this.correctIndex = null;
          this.error = null;
        }
      },

      /*currentQuestion(){*/
        /*this.selectIndex = null;*/
        /*this.correctIndex = null;*/
        /*this.errorIndex = null;*/
      /*}*/
    },
    methods:{
      selectAnswer(index){
        this.selectIndex = index;
      },

      submit(){
        this.correctIndex = this.answers.indexOf(this.currentQuestion.correct_answer);

        if(this.correctIndex === this.selectIndex){
          this.selectIndex = null;
          this.corrects();
        }else{
          this.error = this.selectIndex;
        }

      },
      handleAnswerClass(index){
        if(this.correctIndex === null && this.selectIndex === index){
          return 'info'  
        }else if(this.correctIndex === index){
          return 'success'  
        }else if(this.error === index){
          return 'error'  
        }
      },
    },

  }
</script>
<style>
  .answers{
    cursor: pointer;
    color:black;
  }
  .error{
    background-color: #FC5F5F !important; 
    color:black !important;
  }
  .success{
    background-color: #97F88A !important; 
    color:black !important;
  }
  .info{
    background-color: #53D1C8 !important; 
  }
</style>
