<template>
  <div>
    <h1>Voice Search</h1>
    <button @click="startSpeechRecognition">Start Search</button>
    <div v-if="result !== null">
      <p>You said: {{ result.text }}</p>
      <p>Sentiment: {{ result.sentiment }}</p>
      <div v-if="result.sentiment === 'Positive'" class="container">
        <p>Positive sentiment detected!</p>
        <button class="btnCLick" @click="BiliBiliSearch">Search On blibli</button>
        <button class="btnCLick" @click="AmazonSearch">Search On Amazon</button>
        <button class="btnCLick" @click="FlipkartSearch">Search On Flipkart</button>

      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import axios from 'axios';

export default {
  setup() {
    const result = ref(null);

    const startSpeechRecognition = async () => {
      try {
        const response = await axios.get('http://127.0.0.1:5000/api/start_speech_recognition', {
          headers: {
            'Access-Control-Allow-Origin': '*',
          },
        });
        result.value = response.data;
        console.log(result.value);
        if (result.value.search) {
          window.open(`https://www.blibli.com/cari/${result.value}`, '_blank');
        }
      } catch (error) {
        console.error('Error starting speech recognition:', error);
      }
    };

    const BiliBiliSearch = () => {
      window.open(`https://www.blibli.com/cari/${result.value.text}`, '_blank');

    };

    const AmazonSearch = ()=>{
      window.open(`https://www.amazon.in/s?k=${result.value.text}`, '_blank');
    }

    const FlipkartSearch = ()=>{
      window.open(`https://www.flipkart.com/search?q=${result.value.text}`, '_blank');
    }

    return {
      result,
      startSpeechRecognition,
      BiliBiliSearch,
      AmazonSearch,
      FlipkartSearch
    };
  },
};
</script>

<style>

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.btnCLick{
  margin: 10px;
}


button {
  padding: 10px;
  font-size: 16px;
  cursor: pointer;
}
</style>
