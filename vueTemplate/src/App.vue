<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const message = ref('Loading...')
const message2 = ref('Karyawan should be here');

const nameInput = ref('')
const nikInput = ref('')

//get
const fetchKaryawan = async () => {
  try {
    const response = await axios.get('http://localhost:8080/api/hello')
    const response2 = await axios.get('http://localhost:8080/api/karyawan/get')
    message.value = response.data
    message2.value = response2.data
  } catch (error) {
    message.value = 'Error connecting to backend'
    console.error(error)
  }
}

// post
const handleSubmit = async () => {
  try{
    const payload = {
      name: nameInput.value,
      nik: nikInput.value
    }

    await axios.post('http://localhost:8080/api/karyawan/post', payload)

    //clear
    nameInput.value = ''
    nikInput.value = ''

    //get after post
    await fetchKaryawan()
  }
  catch (error) {
    console.error('Error saving karyawan:', error)
  }
}

//delete
const handleDelete = async (karyawanId) => {
  try{
    await axios.delete(`http://localhost:8080/api/karyawan/delete/${karyawanId}`)
    await fetchKaryawan() // get after delete
  }
  catch (error) {
    console.log('Error deleting karyawan:', error)
  }
}

onMounted(async () => {
  fetchKaryawan()
})
</script>

<template>
  <main style="font-family: sans-serif; text-align: center; margin-top: 50px;">
    <h1>Spring Boot + Vue App</h1>
    <p>{{ message }}</p>
    <!--  add  -->
    <form @submit.prevent="handleSubmit" style="margin-bottom: 30px">
      <input v-model="nameInput" placeholder="Nama Karyawan" required style="margin-right: 10px; padding: 5px;" />
      <input v-model="nikInput" placeholder="NIK" required style="margin-right: 10px; padding: 5px;" />
      <button type="submit" style="padding: 5px 15px;">Add karyawan</button>
    </form>

    <h2>Daftar Karyawan</h2>
    <ul style="list-style: none; padding: 0">
        <li v-for="item in message2" :key="item.id" style="margin: 10px 0;">
          <p>{{ item.name }} + {{item.nik}}</p>
          <button @click="handleDelete(item.id)" style="margin-left: 15px; background-color: #ff4d4d; color: white; border: none; padding: 3px 8px
          ; cursor: pointer;">
            Delete
          </button>
        </li>
    </ul>
  </main>
</template>