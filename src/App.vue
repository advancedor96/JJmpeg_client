

<template>
<v-layout class="rounded rounded-md">
  <v-app-bar title="JJmpeg - 下載 m3u8 串流"></v-app-bar>
  <v-main>

    <v-text-field label="m3u8 URL" class="mt-3" v-model="m3u8Url"></v-text-field>
    
    <v-btn :href="finalUrl">
      Download it!
    </v-btn>


  </v-main>
</v-layout>

</template>
<script setup>
import { ref, computed } from 'vue'


const apiUrl = ref('https://jjmpeg-api.onrender.com/download')
const m3u8Url = ref('')

const finalUrl = computed(()=>{
  const tmp = apiUrl.value + '?url=' + m3u8Url.value;
  console.log('最後的url:',tmp);
  return tmp;
})
</script>
<style scoped>

</style>
