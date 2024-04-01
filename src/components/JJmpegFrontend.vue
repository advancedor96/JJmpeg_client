<template>
  <v-layout class="rounded rounded-md">
    <v-app-bar title="JJmpeg - 下載 m3u8 串流(配合後端)"></v-app-bar>
    <v-main>
      <v-text-field label="狀態" class="mt-3" v-model="status"></v-text-field>
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
  import { ref, computed, onMounted } from 'vue'
  import axios from 'axios'
  import { io } from "socket.io-client";

  
  const apiUrl = ref('https://jjmpeg-api.onrender.com')
  // const apiUrl = ref('http://localhost:3000')
  const socket = io(apiUrl.value);
  const m3u8Url = ref('https://assets.afcdn.com/video49/20210722/m3u8/lld/v_645516.m3u8')
  const Origin = ref('')
  const Referer = ref('')
  const status = ref('');
  
  const doGet = async ()=>{
    try {
      let res = axios.get('https://jjmpeg-api.onrender.com/get');
      console.log('res:',res);
      const socket = new WebSocket('wss://jjmpeg-api.onrender.com');
      socket.onopen = function(event) {
        console.log('ws 連到 server~');
      };

      socket.onmessage = function(event) {
        console.log('收到服务器消息:', event.data);
      };
      
      
    } catch (err) {
      console.log('err:',err);
    }
  }
  onMounted(()=>{

  socket.emit('chat', "FFFront!");
    setInterval(() => {
      getStatus();
    }, 500);
  })
  socket.on('hello', (msg)=>{
    console.log('收到io:',msg);
  })
  const getStatus = async ()=>{
    let res = await axios.get(apiUrl.value + '/status');
    status.value = res.data;
    
  }
  const download = async ()=>{
    try {
      await axios.post(apiUrl.value + '/download', {
        "url": m3u8Url.value,
        "Origin": Origin.value,
        "Referer": Referer.value
      })
      
    } catch (err) {
      console.log('err:',err);
    } finally{
      console.log('finished');
    }
  }

</script>
<style scoped>
  
</style>
  