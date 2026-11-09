<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const message = ref('Loading...')
const message2 = ref('Karyawan should be here');

onMounted(async () => {
  try {
    const response = await axios.get('http://localhost:8080/api/hello')
    const response2 = await axios.get('http://localhost:8080/api/karyawan/get')
    message.value = response.data
    message2.value = response2.data
  } catch (error) {
    message.value = 'Error connecting to backend'
    console.error(error)
  }
})
</script>

<template>
  <main style="font-family: sans-serif; text-align: center; margin-top: 50px;">
    <h1>Spring Boot + Vue App</h1>
    <p>{{ message }}</p>
    <p>{{ message2 }}</p>
  </main>
</template>