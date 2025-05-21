<template>
    <div class="container mx-auto mt-20">
        <Header />
        <div class="mt-12 md:flex">
            <Formulario v-model:nombre="paciente.nombre" v-model:propietario="paciente.propietario"
                v-model:email="paciente.email" v-model:alta="paciente.alta" v-model:sintomas="paciente.sintomas"
                @guardar-paciente="guardarPacientes" :id="paciente.id" />
            <div class="md:w-1/2 md:h-screen overflow-y-scroll">
                <h3 class="font-black text-3xl text-center">Administra tus Pacientes</h3>
                <div v-if="pacientes.length > 0">
                    <p class="text-lg mt-5 text-center mb-10">
                        Información de <span class="text-indigo-600 font-bold">Pacientes</span>
                    </p>
                    <Paciente v-for="(paciente, index) in pacientes" :key="index" :paciente="paciente"
                        @actualizar-paciente="actualizarPaciente" @eliminar-paciente="eliminarPaciente" />
                </div>
                <p v-else class="mt-20 text-2xl text-center">
                    No hay pacientes
                </p>
            </div>
        </div>
    </div>
</template>
<script setup>
import { ref, reactive, onMounted, watch } from "vue";
import { uid } from 'uid'
import Header from "./components/Header.vue";
import Formulario from "./components/Formulario.vue";
import Paciente from "./components/Paciente.vue";
const pacientes = ref([]);
const paciente = reactive({
    id: '',
    nombre: '',
    propietario: '',
    email: '',
    alta: '',
    sintomas: ''
});
onMounted(() => {
    const pacientesStorage = JSON.parse(localStorage.getItem('pacientes')) || [];
    if (pacientesStorage.length > 0) {
        pacientes.value = pacientesStorage;
    }
    pacientes.value = pacientesStorage;
});
watch(pacientes, (newPacientes) => {
    guardarPacientesStorage();
}, { deep: true });

const guardarPacientesStorage = () => {
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value));
};

const guardarPacientes = () => {
    if (paciente.id) {
        const pacienteActualizado = pacientes.value.find(p => p.id === paciente.id);
        if (pacienteActualizado) {
            Object.keys(paciente).forEach(key => pacienteActualizado[key] = paciente[key]);
        }
    } else {
        pacientes.value.push({
            ...paciente,
            id: uid()
        });
    }
    guardarPacientesStorage();
    Object.keys(paciente).forEach(key => paciente[key] = '')
};
const actualizarPaciente = (id) => {
    const pacienteActualizado = pacientes.value.find(paciente => paciente.id === id);
    if (pacienteActualizado) {
        Object.assign(paciente, pacienteActualizado);
    }
};
const eliminarPaciente = (id) => {
    const pacienteIndex = pacientes.value.findIndex(paciente => paciente.id === id);
    if (pacienteIndex !== -1) {
        pacientes.value.splice(pacienteIndex, 1);
    }
};
</script>