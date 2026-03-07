<template>
  <div class="modal-mask">
    <div class="modal-container modal-anim user-form-modal" @click.stop>
      <h2 class="user-form-title">{{ isEditing ? 'Editar usuario' : 'Añadir usuario' }}</h2>
      <form @submit.prevent="submit" class="user-form">
        <div class="user-form-group">
          <label class="user-form-label">Nombre</label>
          <input v-model="user.name" class="user-form-input" required placeholder="Nombre" />
        </div>
        <div class="user-form-group">
          <label class="user-form-label">Usuario</label>
          <input v-model="user.username" class="user-form-input" required placeholder="Usuario" />
        </div>
        <div class="user-form-group">
          <label class="user-form-label">Email</label>
          <input v-model="user.email" class="user-form-input" type="email" required placeholder="Email" />
        </div>
        <div class="user-form-group">
          <label class="user-form-label">Teléfono</label>
          <input v-model="user.phone" class="user-form-input" required placeholder="Teléfono" />
        </div>
        <div class="user-form-actions">
          <button type="button" class="user-form-cancel" @click="close">Cancelar</button>
          <button type="submit" class="user-form-submit">{{ isEditing ? 'Actualizar' : 'Guardar' }}</button>
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
@keyframes modalIn {
  0% {
    opacity: 0;
    transform: scale(0.7);
  }
  100% {
    opacity: 1;
    transform: scale(1);
  }
}
.modal-anim {
  animation: modalIn 0.22s cubic-bezier(.4,1.3,.6,1) both;
}
/* --- Nuevo diseño tipo imagen --- */
.user-form-modal {
  background: #fff;
  border-radius: 16px;
  box-shadow: 0 8px 32px rgba(0,0,0,0.10);
  padding: 2.5rem 2.5rem 2rem 2.5rem;
  min-width: 350px;
  max-width: 420px;
  width: 100%;
}
.user-form-title {
  color: #444;
  font-size: 2rem;
  font-weight: 700;
  margin-bottom: 2rem;
  text-align: left;
}
.user-form {
  display: flex;
  flex-direction: column;
  gap: 1.3rem;
}
.user-form-group {
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
}
.user-form-label {
  font-weight: 700;
  color: #444;
  font-size: 1.1rem;
  margin-bottom: 0.2rem;
  text-align: left;
}
.user-form-input {
  padding: 0.9rem 1.1rem;
  border: 1.5px solid #e0e4ea;
  border-radius: 8px;
  font-size: 1.1rem;
  color: #444;
  background: #f9fafb;
  outline: none;
  transition: border 0.2s;
  box-shadow: 0 2px 8px rgba(0,0,0,0.03);
}
.user-form-input:focus {
  border: 1.5px solid #2bb6a8;
  background: #fff;
}
.user-form-actions {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  gap: 1.5rem;
  margin-top: 1.2rem;
}
.user-form-submit {
  background: #2bb6a8;
  color: #fff;
  font-weight: 700;
  font-size: 1.1rem;
  border: none;
  border-radius: 7px;
  padding: 0.8rem 2.2rem;
  cursor: pointer;
  box-shadow: 0 2px 8px rgba(43,182,168,0.08);
  transition: background 0.18s;
}
.user-form-submit:hover {
  background: #229e92;
}
.user-form-cancel {
  background: #e0e4ea;
  color: #444;
  font-weight: 700;
  font-size: 1.1rem;
  border: none;
  border-radius: 7px;
  padding: 0.8rem 2.2rem;
  cursor: pointer;
  transition: background 0.18s;
}
.user-form-cancel:hover {
  background: #cfd4db;
}
</style>
