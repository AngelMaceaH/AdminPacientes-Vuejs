<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento pacientes</h2>
        <p class="text-lg mt-5 text-center mb-10">
            Añade pacientes y <span class="text-indigo-600 font-bold">Administralos</span>
        </p>
        <form class="bg-white shadow-md rounded-lg py-10 px-5 mb-10" @submit.prevent="validar">
            <Alerta v-if="alerta.mensaje" :alerta="alerta" />
            <div class="mb-5">
                <label for="mascota" class="block text-gray-700 uppercase font-bold">Nombre Mascota</label>
                <input id="mascota" type="text" placeholder="Nombre de la mascota"
                    class="border-1 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    @input="$emit('update:nombre', $event.target.value)" :value="nombre" />
            </div>
            <div class="mb-5">
                <label for="propietario" class="block text-gray-700 uppercase font-bold">Nombre Propietario</label>
                <input id="propietario" type="text" placeholder="Nombre del propietario"
                    class="border-1 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    @input="$emit('update:propietario', $event.target.value)" :value="propietario" />
            </div>
            <div class="mb-5">
                <label for="email" class="block text-gray-700 uppercase font-bold">Email Propietario</label>
                <input id="email" type="email" placeholder="Email del propietario"
                    class="border-1 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    @input="$emit('update:email', $event.target.value)" :value="email" />
            </div>
            <div class="mb-5">
                <label for="alta" class="block text-gray-700 uppercase font-bold">Alta</label>
                <input id="alta" type="date" class="border-1 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    @input="$emit('update:alta', $event.target.value)" :value="alta" />
            </div>
            <div class="mb-5">
                <label for="sintomas" class="block text-gray-700 uppercase font-bold">Sintomas</label>
                <textarea id="sintomas" placeholder="Describe los sintomas"
                    class="border-1 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                    @input="$emit('update:sintomas', $event.target.value)" :value="sintomas"></textarea>
            </div>

            <input type="submit" :value="[editando ? 'Guardar cambios' : 'Registrar Paciente']"
                class=" w-full p-3 text-white uppercase font-bold  cursor-pointer transition-colors" 
                :class="[editando ? 'bg-amber-600 hover:bg-amber-700' : 'bg-indigo-600 hover:bg-indigo-700']"
                />

        </form>
    </div>
</template>

<script setup>
import { reactive, computed } from 'vue';
import Alerta from './Alerta.vue';
const alerta = reactive({
    tipo: '',
    mensaje: ''
});
const emit = defineEmits(['update:nombre', 'update:propietario', 'update:email', 'update:alta', 'update:sintomas', 'guardar-paciente']);
const props = defineProps({
    id: {
        type: [String, null],
        default: ''
    },
    nombre: {
        type: String,
        default: ''
    },
    propietario: {
        type: String,
        default: ''
    },
    email: {
        type: String,
        default: ''
    },
    alta: {
        type: String,
        default: ''
    },
    sintomas: {
        type: String,
        default: ''
    }
});
const validar = () => {
    const { nombre, propietario, email, alta, sintomas } = props;
    if ([nombre, propietario, email, alta, sintomas].includes('')) {
        alerta.mensaje = 'Todos los campos son obligatorios';
        alerta.tipo = 'error';
        return;
    }
    emit('guardar-paciente');
    alerta.mensaje = 'Paciente agregado correctamente';
    alerta.tipo = 'exito';
    setTimeout(() => {
        alerta.mensaje = '';
        alerta.tipo = '';
    }, 3000);
};
const editando = computed(() => {
    return props.id !== '';
});
</script>
