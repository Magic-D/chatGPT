<!-- src/components/ChatComponent.vue -->
<template>
  <div>
    <textarea v-model="userInput" rows="4" cols="50"></textarea>
    <button @click="generateResponse">送信</button>
    <div v-if="response">{{ response }}</div>
  </div>
</template>






<script>
import { ref } from 'vue';
import * as openai from 'openai';

const apiKey = '秘密鍵';

export default {
  name: 'ChatComponent',
  setup() {
    const userInput = ref('');
    const response = ref('');

    async function generateResponse() {
      try {
        const client = new openai.OpenAIApi({ key: apiKey }); // APIクライアントの初期化
        const chatGptResponse = await client.Completions.create({
          prompt: userInput.value,
          max_tokens: 50,
        });

        response.value = chatGptResponse.choices[0].text;
      } catch (error) {
        console.error(error);
      }
    }

    return {
      userInput,
      response,
      generateResponse,
    };
  },
};
</script>
