<template>
  <div style="position: fixed; inset: 0; background: rgba(0, 0, 0, 0.5)">
    <div style="background: gray; padding: 1em; margin: 10% auto; width: 300px">
      <h3>Confirmar</h3>
      <p>{{ message }}</p>
      <button @click="$emit('confirm')">Eliminar</button>
      <button @click="$emit('cancel')">Cancelar</button>
    </div>
  </div>
</template>

<script setup>
defineProps(['message'])
defineEmits(['confirm', 'cancel'])
</script>
