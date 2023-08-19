<script setup>
import { ref, reactive, watch, onMounted } from "vue";
import Header from "./components/Header.vue";
import Formulario from "./components/Formulario.vue";
import Paciente from "./components/Paciente.vue";

const pacientes = ref([]);

const paciente = reactive({
  id: null,
  mascota: "",
  propietario: "",
  email: "",
  fecha_alta: "",
  sintomas: "",
});

watch(pacientes, () => {
  guardarLocalStorage();
}, {
  deep: true // Implementar cuando haya objetos almacenados
});

onMounted(() => {

  // Comprobar si hay pacientes en LocalStorage y asignarlos al array
  const pacientesLS = localStorage.getItem('pacientes');
  
  if(pacientesLS) {
    pacientes.value = JSON.parse(pacientesLS);
  }
})

const guardarLocalStorage = () => {
  localStorage.setItem('pacientes', JSON.stringify(pacientes.value));
}

const guardarPaciente = () => {
  if (paciente.id) {
    const { id } = paciente;

    const indice = pacientes.value.findIndex(
      (pacienteState) => pacienteState.id === id
    );
    pacientes.value[indice] = { ...paciente };
  } else {
    pacientes.value.push({
      ...paciente,
      id: self.crypto.randomUUID(),
    });
  }

  // Reiniciar el objeto
  paciente.mascota = "";
  paciente.propietario = "";
  paciente.email = "";
  paciente.fecha_alta = "";
  paciente.sintomas = "";
  paciente.id = "";
};

const actualizarPaciente = (id) => {
  const pacienteEditar = pacientes.value.find(
    (pacienteState) => pacienteState.id === id
  );
  console.log(pacienteEditar);

  // Asignar valores del paciente al reactivo
  Object.assign(paciente, pacienteEditar);
};

const eliminarPaciente = (id) => {
  pacientes.value = pacientes.value.filter(pacienteState => pacienteState.id !== id);
}

</script>

<template>
  <div class="container mx-auto mt-20 mb-14">
    <Header />

    <div class="w-11/12 mx-auto mt-12 md:w-full md:flex">
      <Formulario
        v-model:mascota="paciente.mascota"
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email"
        v-model:fecha_alta="paciente.fecha_alta"
        v-model:sintomas="paciente.sintomas"
        :id="paciente.id"
        @guardar-paciente="guardarPaciente"
      />

      <div class="md:w-1/2">
        <h3 class="font-black text-3xl text-center">
          Administra tus Pacientes
        </h3>

        <p class="text-lg mt-5 text-center mb-10">
          Gestiona tus
          <span class="text-indigo-600 font-bold">Pacientes y Citas</span>
        </p>

        <!-- Listado de Pacientes -->
        <div v-if="pacientes.length > 0">
          <Paciente
            v-for="paciente in pacientes"
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />
        </div>

        <p v-else class="mt-20 text-2xl text-center">¡Todavía no hay pacientes agregados!</p>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
