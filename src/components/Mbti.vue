<template>
  <v-app>
    <v-container>
      <v-card class="mx-auto" max-width="600">
        <v-card-title class="headline text-center">MBTI 测试</v-card-title>
        <v-progress-linear
            :model-value="progress"
            height="10"
            color="blue"
            class="mb-4"
        ></v-progress-linear>
        <v-card-text>
          <div v-if="!completed">
            <div v-if="currentQuestion" class="question">
              <p>{{ currentQuestion.text }}</p>
              <v-btn
                  @click="answerQuestion('A')"
                  color="primary"
                  class="mx-2 my-1"
              >
                {{ currentQuestion.a }}
              </v-btn>
              <v-btn
                  @click="answerQuestion('B')"
                  color="primary"
                  class="mx-2 my-1"
              >
                {{ currentQuestion.b }}
              </v-btn>
            </div>
            <div v-else>
              <p>Loading questions...</p>
            </div>
          </div>
          <div v-else class="result">
            <v-alert type="success" class="mt-4">
              你的MBTI类型是: {{ mbtiType }}
            </v-alert>
            <v-img :src="mbtiDetails.image" class="mb-4" max-height="400px" contain></v-img>
            <v-card class="description-card">
              <v-card-text class="description-text">{{ mbtiDetails.description }}</v-card-text>
            </v-card>
          </div>
        </v-card-text>
      </v-card>
    </v-container>
  </v-app>
</template>

<script>
import { ref, computed } from 'vue';
import { questions, mbtiMapping } from '@/mbtiData';

export default {
  name: 'MbtiTest',
  setup() {
    const currentQuestionIndex = ref(0);
    const answers = ref([]);
    const completed = ref(false);
    const mbtiType = ref('');
    const mbtiDetails = ref({ image: '', description: '' });

    const progress = computed(() => {
      return ((currentQuestionIndex.value + 1) / questions.length) * 100;
    });

    const currentQuestion = computed(() => {
      return questions[currentQuestionIndex.value];
    });

    const answerQuestion = (choice) => {
      answers.value.push({ index: currentQuestionIndex.value, choice });
      if (currentQuestionIndex.value < questions.length - 1) {
        currentQuestionIndex.value++;
      } else {
        completed.value = true;
        calculateResult();
      }
    };

    const calculateResult = () => {
      // 假设结果为 ESTJ
      mbtiType.value = 'ESTJ';
      mbtiDetails.value = mbtiMapping[mbtiType.value];
    };

    return {
      currentQuestion,
      completed,
      mbtiType,
      mbtiDetails,
      answerQuestion,
      progress,
    };
  },
};
</script>

<style scoped>
.v-btn {
  min-width: 120px;
}
.result {
  text-align: center;
}
.result .v-alert {
  font-size: 1.2em;
  font-weight: bold;
}
.result .v-img {
  max-width: 100%;
  height: auto;
}
.description-card {
  margin-top: 20px;
}
.description-text {
  font-size: 1.1em;
  line-height: 1.6em;
}
</style>
