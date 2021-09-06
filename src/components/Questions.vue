<template>
  <div class="questions-ctr">
    <div class="progress">
      <div class="bar" :style="{width:`${(noOfQuestionAnswered/totalQuestions)*100}%`}"></div>
      <div class="status" v-text="status"></div>
    </div>
    <div class="single-question" 
    v-for="(question,_,quesIndex) in questions" 
    :key="quesIndex"
    >
    
      <div class="question" @click="$emit('selectQuestion',quesIndex)">
        {{ question.q }}
      </div>
      <div class="answers">

        <div class="answer" 
          v-for="(answer,ansIndex) in question.answers"
          v-show="quesIndex === currentQuestion"
          :key="ansIndex"
          @click.once="$emit('reportAnswer', answer.is_correct ,quesIndex,ansIndex)"
        >
        {{ answer.text }}
        </div>

      </div>
      <br v-if="((totalQuestions-1) !== quesIndex)">
    
    </div>
  </div>
</template>

<script lang="ts">import { defineComponent } from "@vue/runtime-core";
import { PropType } from "vue";

interface Question {
  [key:string]:{
    q: string,
    answers: {
    text: string,
    is_correct: boolean
    }[]
  }
}



export default defineComponent({
  name: "Question",
  props: {
    totalQuestions: {
      type: Number,
      required: true
    },
    noOfQuestionAnswered: {
      type: Number,
      required: true
    },
    questions: {
      type: Object as PropType<Question>,
      required:true
    },
    currentQuestion:{
      type:Number,
      required:true
    }
  },
  emits:{
    reportAnswer:(isCorrect:boolean,questionIndex:number,ansIndex:number)=>{
      return isCorrect!==undefined && questionIndex!==undefined
    },
    selectQuestion:(index:number)=>{
      return index!==undefined
    }
  },
  computed:{
    status(){
      return `${this.noOfQuestionAnswered} out of ${this.totalQuestions} questions answered`
    }
  }
})
</script>