<script setup>
// Para poder hacer objetos reactivos
import { ref } from 'vue'
import { getDataChanged_collection, deleteData, deleteField } from '@/firebase'
import FormularioSensor from '@/components/FormularioSensor.vue'
import FormularioEjecutor from '@/components/FormularioEjecutor.vue'
const referencia = 'IOT_ALV'
const cambios = ref([])
const mostrarFormularioSensor = ref(false)
const mostrarFormularioEjecutor = ref(false)
let idActivo = ''
const sensor = ref('')
const datoMedido = ref('')
const ejecutor = ref('')
const checked = ref(false)
//
const ocultarFormularioSensor = () => {
  mostrarFormularioSensor.value = false
}
//
const ocultarFormularioEjecutor = () => {
  mostrarFormularioEjecutor.value = false
}
// Eliminar documento
const eliminar = (id) => {
  deleteData(id, referencia)
}
//
const eliminarComponente = (key, id) => {
  deleteField(id, referencia, key)
}
//
const addSensor = (id) => {
  idActivo = id
  sensor.value = ''
  datoMedido.value = ''
  mostrarFormularioSensor.value = true
  mostrarFormularioEjecutor.value = false
}
//
const addEjecutor = (id) => {
  idActivo = id
  ejecutor.value = ''
  checked.value = false
  mostrarFormularioEjecutor.value = true
  mostrarFormularioSensor.value = false
}
// Para imprimir la colección
getDataChanged_collection(referencia, (querySnapshot) => {
  cambios.value = []
  querySnapshot.forEach((doc) => {
    cambios.value.push({ id: doc.id, datos: doc.data() })
  })
})
</script>
<template>
  <ul>
    <FormularioSensor
      :idActivo="idActivo"
      :mostrarFormularioSensor="mostrarFormularioSensor"
      @cancelarClick="ocultarFormularioSensor"
    />
    <FormularioEjecutor
      :idActivo="idActivo"
      :mostrarFormularioEjecutor="mostrarFormularioEjecutor"
      @cancelarClick="ocultarFormularioEjecutor"
    />
    <li
      class="border border-black border-solid bg-blue-500 text-white font-bold text-lg m-2.5 p-1 text-center"
      v-for="(cambio, index) in cambios"
      :key="index"
      :id="cambio.id"
    >
      <p>
        Espacio: <span class="text-rose-800">{{ cambio.id }}</span>
      </p>
      <ul>
        <li
          class="border border-black border-solid bg-blue-500 text-white font-bold text-lg m-2.5 p-1 text-center"
          v-for="(value, key) in cambio.datos"
          :key="key"
        >
          {{ key }}: {{ value }}
          <button class="m-1.5 bg-sky-900 p-1" @click.stop="eliminarComponente(key, cambio.id)">
            Eliminar componente
          </button>
        </li>
      </ul>
      <button class="m-1.5 bg-sky-900 p-1" @click.stop="eliminar(cambio.id)">
        Eliminar espacio
      </button>
      <button class="m-1.5 bg-sky-900 p-1" @click.stop="addSensor(cambio.id)">
        Añadir/Actualizar sensor
      </button>
      <button class="m-1.5 bg-sky-900 p-1" @click.stop="addEjecutor(cambio.id)">
        Añadir/Actualizar ejecutor
      </button>
    </li>
  </ul>
</template>
<style scoped></style>
