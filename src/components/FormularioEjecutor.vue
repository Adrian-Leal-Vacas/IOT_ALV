<script setup>
import { ref, defineProps, defineEmits } from 'vue'
import { updateData } from '@/firebase'
const referencia = 'IOT_ALV'
const props = defineProps(['idActivo', 'mostrarFormularioEjecutor'])
const emits = defineEmits(['cancelarClick'])
const ejecutor = ref('')
const checked = ref(false)
//
const submitForm = () => {
  updateData(props.idActivo, referencia, {
    [ejecutor.value]: checked.value
  })
  ejecutor.value = ''
  emits('cancelarClick')
  alert('Sensor actualizado correctamente')
}
//
const enviarCancelarClick = () => {
  if (props.mostrarFormularioEjecutor) {
    emits('cancelarClick')
  }
}
</script>
<template>
  <form class="mb-5 text-base" v-if="mostrarFormularioEjecutor" @submit.prevent="submitForm">
    <select v-model="ejecutor" name="ejecutor">
      <option disabled value="">Selecciona un ejecutor</option>
      <option>Bajar persianas</option>
      <option>Encender luz</option>
      <option>Abrir ventanas</option>
    </select>
    <label class="ml-2.5 text-base text-slate-700" for="checkbox">Activar</label>
    <input
      class="appearance-none w-5 h-5 border-2 border-white border-solid rounded outline-none cursor-pointer ml-1.5 transition checked:bg-green-600"
      v-model="checked"
      id="checkbox"
      type="checkbox"
      name="checkbox"
    />
    <button class="m-1.5 p-1" type="submit">Grabar</button>
    <button class="m-1.5 p-1" type="button" @click="enviarCancelarClick">Cancelar</button>
  </form>
</template>
<style lang="scss" scoped></style>
