<script setup>
import { reactive, computed } from 'vue';

const emit = defineEmits(['submit:answer', 'quizFinished']);
const questions = [
  {
    question: 'What is the capital of France?',
    options: ['London', 'Berlin', 'Paris', 'Rome'],
    answer: 'Paris',
  },
  {
    question: 'Which planet is closest to the sun?',
    options: ['Earth', 'Mars', 'Venus', 'Mercury'],
    answer: 'Mercury',
  },
];

const state = reactive({
  step: 0,
  answer: '',
  isCorrect: false,
  showResult: false,
});

const question = computed(() => questions[state.step].question);
const options = computed(() => questions[state.step].options);
const correctAnswer = computed(() => questions[state.step].answer);
const isQuizFinished = computed(() => questions.length === state.step + 1);

const onSubmit = () => {
  if (state.showResult) {
    goToNextStep();
    return;
  }
  state.isCorrect = state.answer === correctAnswer.value;
  state.showResult = true;
};

const goToNextStep = () => {
  if (isQuizFinished.value) {
    emit('submit:answer', state.isCorrect);
    emit('quizFinished', questions.length);
    return;
  }
  emit('submit:answer', state.isCorrect);
  state.answer = '';
  state.isCorrect = false;
  state.showResult = false;
  state.step++;
};
</script>
<template>
  <form @submit.prevent="onSubmit" class="text-white">
    <h3 class="text-2xl pb-2 mb-4 border-b border-white">{{ question }}</h3>
    <div v-for="(option, index) in options" :key="index" class="mb-2">
      <input
        type="radio"
        :id="option"
        :value="option"
        v-model="state.answer"
        class="mr-4"
      />
      <label :for="option" class="text-lg">{{ option }}</label>
    </div>
    <div class="flex justify-between items-center mt-8">
      <div
        v-show="true || state.showResult"
        class="w-full mr-6 text-center line py-1 border"
        :class="[
          state.isCorrect
            ? 'border-green-500 bg-green-500/30 text-green-500'
            : 'border-red-500 bg-red-500/30 text-red-500',
        ]"
      >
        {{ state.isCorrect ? 'Correct!' : 'Incorrect!' }}
      </div>
      <button class="bg-blue-500 hover:bg-blue-600 disabled:bg-gray-500 rounded py-1 w-24" :disabled="!state.answer">{{ state.showResult ? 'Next' : 'Submit' }}</button>
    </div>
  </form>
</template>
