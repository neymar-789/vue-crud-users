<template>
  <div class="modal-mask" @click.self="close">
    <div class="modal-container">
      <h2>{{ isEditing ? 'Editar usuario' : 'Añadir usuario' }}</h2>
      <form @submit.prevent="submit">
        <div class="form-group">
          <label>Nombre</label>
          <input v-model="user.name" required />
        </div>
        <div class="form-group">
          <label>Usuario</label>
          <input v-model="user.username" required />
        </div>
        <div class="form-group">
          <label>Email</label>
          <input v-model="user.email" type="email" required />
        </div>
        <div class="form-group">
          <label>Teléfono</label>
          <input v-model="user.phone" required />
        </div>
        <div class="modal-actions">
          <button type="button" @click="close">Cancelar</button>
          <button type="submit" class="btn-primary">{{ isEditing ? 'Actualizar' : 'Guardar' }}</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { reactive, computed, watch } from 'vue'
const emit = defineEmits(['save', 'close'])
const props = defineProps({
  show: Boolean,
  userToEdit: Object
})
const user = reactive({
  id: null,
  name: '',
  username: '',
  email: '',
  phone: ''
})
const isEditing = computed(() => !!props.userToEdit?.id)
// Cargar los datos del usuario si se está editando
watch(() => props.userToEdit, (newUser) => {
  if (newUser) {
    user.id = newUser.id
    user.name = newUser.name
    user.username = newUser.username
    user.email = newUser.email
    user.phone = newUser.phone
  }
}, { immediate: true })
function submit() {
  // Validación simple de email
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!emailRegex.test(user.email)) {
    alert('Email no válido')
    return
  }
  emit('save', { ...user })
  reset()
}
function close() {
  emit('close')
  reset()
}
function reset() {
  user.id = null
  user.name = ''
  user.username = ''
  user.email = ''
  user.phone = ''
}
</script>

<style scoped>
.modal-mask {
  position: fixed;
  z-index: 9999;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0,0,0,0.3);
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal-container {
  background: #fff;
  padding: 2rem;
  border-radius: 8px;
  min-width: 350px;
  max-width: 90vw;
  box-shadow: 0 2px 8px rgba(0,0,0,0.15);
}
.form-group {
  margin-bottom: 1rem;
  display: flex;
  flex-direction: column;
}
label {
  font-weight: 500;
  margin-bottom: 0.2rem;
  color: #111;
}
input {
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  color: #111;
  background: #fff;
}
.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 1rem;
}
.btn-primary {
  background: #2196f3;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 0.5rem 1.2rem;
  font-weight: 500;
  cursor: pointer;
}
</style>
