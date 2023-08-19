<script setup>
import { reactive, computed } from "vue";
import Alerta from "./Alerta.vue";

const alerta = reactive({
  tipo: "",
  mensaje: "",
});

const emit = defineEmits([
  "update:mascota",
  "update:propietario",
  "update:email",
  "update:fecha_alta",
  "update:sintomas",
  "guardar-paciente",
]);

const props = defineProps({
  mascota: {
    type: String,
    required: true,
  },
  propietario: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
  },
  fecha_alta: {
    type: String,
    required: true,
  },
  sintomas: {
    type: String,
    required: true,
  },
  id: {
    type: [String, null], // Puede ser String o null
    required: true,
  },
});

const validar = () => {
  // Comprobar si hay campos vacíos
  const { mascota, propietario, email, fecha_alta, sintomas} = props;

  if ([mascota, propietario, email, fecha_alta, sintomas].includes("")) {
    alerta.mensaje = "Todos los campos son obligatorios";
    alerta.tipo = "error";
    return;
  }

  emit("guardar-paciente");

  // Crear una alerta y eliminarla tras 3 segundos
  alerta.mensaje = '¡Paciente guardado con éxito!';
  alerta.tipo = 'exito';

  setTimeout(() => {
    alerta.mensaje = "";
    alerta.tipo = "";
  }, 3000);
};

const editando = computed(() => {
  return props.id 
});

</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Seguimiento de Pacientes</h2>

    <p class="text-lg mt-5 text-center mb-10">
      Añade Pacientes y
      <span class="text-indigo-600 font-bold">Adminístralos</span>
    </p>

    <Alerta v-if="alerta.mensaje" :alerta="alerta" />

    <form
      @submit.prevent="validar"
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
    >
      <!-- Nombre de la mascota -->
      <div class="mb-5">
        <label for="mascota" class="block text-gray-700 uppercase font-bold"
          >Mascota</label
        >

        <input
          id="mascota"
          type="text"
          placeholder="Nombre de la Mascota"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="mascota"
          @input="$emit('update:mascota', $event.target.value)"
        />
      </div>

      <!-- Propietario -->
      <div class="mb-5">
        <label for="propietario" class="block text-gray-700 uppercase font-bold"
          >Propietario</label
        >

        <input
          id="propietario"
          type="text"
          placeholder="Nombre del Propietario"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="propietario"
          @input="$emit('update:propietario', $event.target.value)"
        />
      </div>

      <!-- Correo Electrónico -->
      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold"
          >Correo Electrónico</label
        >

        <input
          id="email"
          type="text"
          placeholder="Correo Electrónico del Propietario"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
        />
      </div>

      <!-- Fecha de Alta -->
      <div class="mb-5">
        <label for="fecha_alta" class="block text-gray-700 uppercase font-bold"
          >Fecha de Alta</label
        >

        <input
          id="fecha_alta"
          type="date"
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="fecha_alta"
          @input="$emit('update:fecha_alta', $event.target.value)"
        />
      </div>

      <!-- Síntomas -->
      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 uppercase font-bold"
          >Síntomas</label
        >

        <textarea
          id="sintomas"
          rows="3"
          placeholder="Describe los síntomas aquí..."
          class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
          :value="sintomas"
          @input="$emit('update:sintomas', $event.target.value)"
        />
      </div>

      <!-- Botón -->
      <input
        type="submit"
        class="w-full p-3 cursor-pointer rounded-md bg-indigo-700 hover:bg-indigo-800 transition-colors text-white font-bold uppercase"
        :value="[editando ? 'Editar Paciente' : 'Registrar Paciente']"
      />
    </form>
  </div>
</template>
