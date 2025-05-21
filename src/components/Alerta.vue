<template>
    <div>
        <p class="text-white text-center font-bold uppercase p-3 rounded-lg mb-10"
            :class="[isError ? 'bg-red-500' : 'bg-green-500']">
            <span class="font-black">
                {{ alerta.mensaje }}
            </span>
        </p>
    </div>
</template>

<script setup>
import { computed } from 'vue';
const props = defineProps({
    alerta: {
        type: Object,
        required: true
    }
});
const isError = computed(() => {
    return props.alerta.tipo === 'error';
});

</script>
