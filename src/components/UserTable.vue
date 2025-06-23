<template>
  <table border="1" cellspacing="0" cellpadding="6">
    <thead>
      <tr>
        <th>Nombre</th>
        <th>Usuario</th>
        <th>Email</th>
        <th>Teléfono</th>
        <th>Acciones</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="user in users" :key="user.id">
        <td>{{ user.name }}</td>
        <td>{{ user.username }}</td>
        <td>{{ user.email }}</td>
        <td>{{ user.phone }}</td>
        <td>
          <button @click="$emit('edit', user)">Editar</button>
          <button @click="$emit('delete', user.id)">Eliminar</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
defineProps(['users'])
defineEmits(['edit', 'delete'])
</script>
