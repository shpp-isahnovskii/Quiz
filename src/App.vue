<script setup>
import { reactive } from 'vue';
import Quiz from '@/Quiz.vue';

const state = reactive({
  totalScore: 0,
  amountOfQuestions: 0,
  finished: false,
});

const onSubmit = (isCorrect) => {
  state.totalScore += +isCorrect; //transform boolean to 1 or 0
};
const showTotalResult = (amountOfQuestions) => {
  state.amountOfQuestions = amountOfQuestions;
  state.finished = true;
};
</script>
<template>
  <section class="w-full h-screen bg-black/80 flex justify-center pt-36">
    <Quiz
      v-if="!state.finished"
      @submit:answer="onSubmit"
      @quiz-finished="showTotalResult"
    />
    <h3 v-else class="text-white">Total score is {{ state.totalScore }} of {{ state.amountOfQuestions }}</h3>
  </section>
</template>
