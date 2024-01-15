<script setup>
import { saveData } from '@/firebase'
import { ref } from 'vue'
const referencia = 'IOT_ALV'
const nombreRoom = ref('')
// Cuando le doy al enviar en el formulario
const submitForm = async () => {
  const documentName = nombreRoom.value.toUpperCase()
  await saveData(referencia, documentName, {})
  nombreRoom.value = ''
}
</script>
<template>
  <form class="mb-5 text-base" @submit.prevent="submitForm">
    <input v-model="nombreRoom" type="text" placeholder="Nombre del espacio" required />
    <button class="m-1.5 p-1" type="submit">Grabar</button>
  </form>
</template>
<style scoped></style>
