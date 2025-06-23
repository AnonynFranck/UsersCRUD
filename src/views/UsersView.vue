<template>
  <div>
    <h2>Gestión de Usuarios</h2>

    <button @click="openCreateModal">Agregar Usuario</button>

    <UserTable :users="users" @edit="openEditModal" @delete="confirmDeleteUser" />

    <UserForm
      v-if="showForm"
      :user="selectedUser"
      :is-editing="isEditing"
      @close="closeForm"
      @save="saveUser"
    />

    <ConfirmDialog
      v-if="showConfirm"
      message="¿Estás seguro que deseas eliminar este usuario?"
      @confirm="deleteUser"
      @cancel="showConfirm = false"
    />

    <p v-if="loading">Cargando usuarios...</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import UserTable from '../components/UserTable.vue'
import UserForm from '../components/UserForm.vue'
import ConfirmDialog from '../components/ConfirmDialog.vue'

const users = ref([])
const loading = ref(false)

const showForm = ref(false)
const showConfirm = ref(false)
const selectedUser = ref(null)
const isEditing = ref(false)
let userToDeleteId = null

const fetchUsers = async () => {
  loading.value = true
  const res = await fetch('https://jsonplaceholder.typicode.com/users')
  const data = await res.json()
  users.value = data
  loading.value = false
}

onMounted(fetchUsers)

const openCreateModal = () => {
  selectedUser.value = null
  isEditing.value = false
  showForm.value = true
}

const openEditModal = (user) => {
  selectedUser.value = { ...user }
  isEditing.value = true
  showForm.value = true
}

const saveUser = (user) => {
  if (isEditing.value) {
    const index = users.value.findIndex((u) => u.id === user.id)
    if (index !== -1) users.value[index] = user
  } else {
    const nextId = Math.max(...users.value.map((u) => u.id)) + 1
    users.value.push({ ...user, id: nextId })
  }
  closeForm()
}

const confirmDeleteUser = (userId) => {
  userToDeleteId = userId
  showConfirm.value = true
}

const deleteUser = () => {
  users.value = users.value.filter((u) => u.id !== userToDeleteId)
  showConfirm.value = false
}

const closeForm = () => {
  showForm.value = false
}
</script>
