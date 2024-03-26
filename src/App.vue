

<template>
<v-layout class="rounded rounded-md">
  <v-app-bar title="JJmpeg - 下載 m3u8 串流"></v-app-bar>
  <v-main>

    <v-text-field label="m3u8 URL" class="mt-3" v-model="m3u8Url"></v-text-field>

    <v-text-field label="Origin" class="mt-3" v-model="Origin"></v-text-field>
    <v-text-field label="Referer" class="mt-3" v-model="Referer"></v-text-field>
    
    <v-btn @click="download">
      Download it!
    </v-btn>


  </v-main>
</v-layout>

</template>
<script setup>
import { ref, computed } from 'vue'
import axios from 'axios'


const apiUrl = ref('https://jjmpeg-api.onrender.com/download')
// const apiUrl = ref('http://localhost:3000/download')
const m3u8Url = ref('')
const Origin = ref('')
const Referer = ref('')

const download = async ()=>{
  try {
    await axios.post(apiUrl.value, {
      url: m3u8Url.value,
      "Origin": Origin.value,
      "Referer": Referer.value
    })
    
  } catch (err) {
    console.log('err:',err);
  } finally{
    console.log('finished');
  }
}
const finalUrl = computed(()=>{
  const tmp = apiUrl.value + '?url=' + m3u8Url.value;
  console.log('最後的url:',tmp);
  return tmp;
})
</script>
<style scoped>

</style>
