<template>
  <div style="position: fixed; inset: 0; background: rgba(0, 0, 0, 0.5)">
    <div style="background: gray; padding: 1em; margin: 10% auto; width: 300px">
      <h3>{{ isEditing ? 'Editar Usuario' : 'Agregar Usuario' }}</h3>
      <form @submit.prevent="handleSubmit">
        <label>Nombre:</label><br />
        <input v-model="form.name" required /><br /><br />
        <label>Usuario:</label><br />
        <input v-model="form.username" required /><br /><br />
        <label>Email:</label><br />
        <input type="email" v-model="form.email" required /><br /><br />
        <label>Teléfono:</label><br />
        <input v-model="form.phone" required /><br /><br />

        <button type="submit">Guardar</button>
        <button type="button" @click="$emit('close')">Cancelar</button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps(['user', 'isEditing'])
const emit = defineEmits(['save', 'close'])

const form = ref({
  name: '',
  username: '',
  email: '',
  phone: '',
})

watch(
  () => props.user,
  (newUser) => {
    if (newUser) {
      form.value = { ...newUser }
    } else {
      form.value = { name: '', username: '', email: '', phone: '' }
    }
  },
  { immediate: true },
)

const handleSubmit = () => {
  emit('save', { ...form.value, id: props.user?.id })
}
</script>
