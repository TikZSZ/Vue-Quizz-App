<template>
  <div class="ctr">
    <Transition name="fade" mode="out-in">
      <Question
        v-if="!isAllAnswered"
        :no-of-question-answered="totalQuestionsAnswered"
        :total-questions="Object.keys(questions).length"
        :current-question="currentQuestion"
        :questions="questions"
        @report-answer="checkAnswer"
        @select-question="selectQuestion"
      />
        <div v-else>
          <Result
          :result-title="calculateResults.title"
          :result-desc="calculateResults.desc"
          :correct-answered="correctAnswers"
          />
          <ResultQuestions
          :questions="analysis" />
          <button type="button" @click="reset" class="reset-btn">Reset</button>
        </div>
      </Transition>
    
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Question from "./components/Questions.vue";
import Result from "./components/Result.vue";
import ResultQuestions,{ AnsweredQuestions} from "./components/ResultQuestions.vue";

export default defineComponent({
  name: "App",
  components: {
    Question,
    Result,
    ResultQuestions
  },
  data() {
    return {
      totalQuestionsAnswered: 0,
      correctAnswers: 0,
      questions: {
        0: {
          q: "What is 2 + 2?",
          answers: [
            {
              text: "4",
              is_correct: true,
            },
            {
              text: "3",
              is_correct: false,
            },
            {
              text: "Fish",
              is_correct: false,
            },
            {
              text: "5",
              is_correct: false,
            },
          ],
        },
        1: {
          q: 'How many letters are in the word "Banana"?',
          answers: [
            {
              text: "5",
              is_correct: false,
            },
            {
              text: "7",
              is_correct: false,
            },
            {
              text: "6",
              is_correct: true,
            },
            {
              text: "12",
              is_correct: false,
            },
          ],
        },
        2: {
          q: "Find the missing letter: C_ke",
          answers: [
            {
              text: "e",
              is_correct: false,
            },
            {
              text: "a",
              is_correct: true,
            },
            {
              text: "i",
              is_correct: false,
            },
          ],
        },
      },
      results: [
        {
          min: 0,
          max: 2,
          title: "Try again!",
          desc: "Do a little more studying and you may succeed!",
        },
        {
          min: 3,
          max: 3,
          title: "Wow, you're a genius!",
          desc: "Studying has definitely paid off for you!",
        },
      ],
      currentQuestion: 0,
      questionsAnswered: {} as {
        [key: string]: {
          selectedAnswerIndex: number;
        };
      },
    };
  },
  methods: {
    checkAnswer(
      isCorrect: boolean,
      questionIndex: number,
      answerIndex: number
    ) {
      this.totalQuestionsAnswered++;
      if (isCorrect) {
        this.correctAnswers++;
      }

      this.questionsAnswered[questionIndex] = {
        selectedAnswerIndex: answerIndex,
      };

      const unAnsweredQuestions = Object.keys(this.questions)
        .map((key) => {
          if (this.questionsAnswered[key] === undefined) {
            return parseInt(key);
          }
        })
        .filter((index) => index !== undefined);

      this.currentQuestion = unAnsweredQuestions[0] as number;
    },
    reset() {
      this.correctAnswers = 0;
      this.totalQuestionsAnswered = 0;
      this.questionsAnswered = {};
      this.currentQuestion = 0;
    },
    selectQuestion(index: number) {
      this.currentQuestion = index;
    },
  },
  computed: {
    isAllAnswered(): boolean {
      if (this.totalQuestionsAnswered < Object.keys(this.questions).length) {
        return false;
      }
      return true;
    },
    calculateResults(): { title: string; desc: string } {
      const { title, desc } = this.results.filter((result) => {
        return (
          result.min <= this.correctAnswers && this.correctAnswers <= result.max
        );
      })[0];
      return { title, desc };
    },
    analysis() {
      const tempObject:AnsweredQuestions = {}
      Object.values(this.questions).map((ques, index) => {
        
          tempObject[index] = {
            ...ques,
            selectedAnswer: this.questionsAnswered[index].selectedAnswerIndex,
          }
      });
      console.log(tempObject)
      return tempObject
    },
  },
});
</script>
