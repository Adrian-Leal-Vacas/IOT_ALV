<script setup>
import { ref, defineProps, defineEmits } from 'vue'
import { updateData } from '@/firebase'
const referencia = 'IOT_ALV'
const props = defineProps(['idActivo', 'mostrarFormularioSensor'])
const emits = defineEmits(['cancelarClick'])
const sensor = ref('')
const datoMedido = ref('')
//
const submitForm = () => {
  updateData(props.idActivo, referencia, {
    [sensor.value]: datoMedido.value
  })
  sensor.value = ''
  emits('cancelarClick')
  alert('Sensor actualizado correctamente')
}
//
const enviarCancelarClick = () => {
  if (props.mostrarFormularioSensor) {
    emits('cancelarClick')
  }
}
</script>
<template>
  <form class="mb-5 text-base" v-if="mostrarFormularioSensor" @submit.prevent="submitForm">
    <select v-model="sensor" name="sensor">
      <option disabled value="">Selecciona un sensor</option>
      <option>Temperatura</option>
      <option>Ruido</option>
      <option>Humedad</option>
    </select>
    <input v-model="datoMedido" type="text" placeholder="Dato medido" required />
    <button class="m-1.5 p-1" type="submit">Grabar</button>
    <button class="m-1.5 p-1" type="button" @click="enviarCancelarClick">Cancelar</button>
  </form>
</template>
<style lang="scss" scoped></style>
