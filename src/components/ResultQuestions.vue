<template>
  <div class="questions-ctr">
    <div
      class="single-question"
      v-for="(question, _, quesIndex) in questions"
      :key="quesIndex"
    >
      <div class="question">
        {{ question.q }}
      </div>
      <div class="answers">
        <div
          class="answer"
          v-for="(answer, ansIndex) in question.answers"
          :key="ansIndex"
          :style="
            decideColor(question.selectedAnswer, ansIndex, answer.is_correct)
          "
        >
          {{
            getText(
              answer.text,
              question.selectedAnswer,
              ansIndex,
              answer.is_correct
            )
          }}
        </div>
      </div>
      <br />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "@vue/runtime-core";
import { PropType } from "vue";

export interface AnsweredQuestions {
  [key: string]: {
    q: string;
    answers: {
      text: string;
      is_correct: boolean;
    }[];
    selectedAnswer: number;
  };
}

export default defineComponent({
  name: "ResultQuestion",
  props: {
    questions: {
      type: Object as PropType<AnsweredQuestions>,
      required: true,
    },
  },
  methods: {
    decideColor(
      selectedAnswer: number,
      currentAnswer: number,
      correctAnswer: boolean
    ) {
      let color: string = "white";
      if (correctAnswer) {
        color = "#C2FFD9";
      }
      if (currentAnswer === selectedAnswer && correctAnswer) {
        color = "#80ED99";
      } else if (currentAnswer === selectedAnswer && !correctAnswer) {
        color = "#FF2442";
      }
      return {
        backgroundColor: color,
        color: "black",
      };
    },
    getText(
      text: string,
      selectedAnswer: number,
      currentAnswer: number,
      correctAnswer: boolean
    ) {
      let decidedText: string = "";
      if (correctAnswer) {
        decidedText = `> This is correct answer`;
      }
      if (currentAnswer === selectedAnswer && correctAnswer) {
        decidedText = `> Correct Answer`;
      } else if (currentAnswer === selectedAnswer && !correctAnswer) {
        decidedText = `> Wrong Answer`;
      }
      return `${text}  ${decidedText}`;
    },
  },
});
</script>
