<script setup>
import { computed } from 'vue';

const props = defineProps({
  alerta: {
    type: Object,
    required: true,
  },
});

const isError = computed(() => {
    return props.alerta.tipo === 'error';
})
</script>

<template>
  <div
    :class="[isError ? 'from-red-500 to-red-700' : 'from-green-500 to-green-700']"
    class="font-bold text-white text-center p-3 uppercase rounded-md mb-3 bg-gradient-to-tr"
  >
    {{ alerta.mensaje }}
  </div>
</template>
