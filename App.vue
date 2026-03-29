<script setup>
import { ref, computed } from 'vue'

const name = ref('')
const email = ref('')
const message = ref('')

const errors = ref({})
const isSubmitted = ref(false)
const isLoading = ref(false)

// ✅ Simple validation
function validate() {
  const newErrors = {}

  if (!name.value) newErrors.name = 'Name is required'
  if (!email.value) {
    newErrors.email = 'Email is required'
  } else if (!email.value.includes('@')) {
    newErrors.email = 'Enter a valid email'
  }
  if (!message.value) newErrors.message = 'Message is required'

  errors.value = newErrors

  return Object.keys(newErrors).length === 0
}

// ✅ Disable button if empty
const isFormValid = computed(() => {
  return name.value && email.value && message.value
})

async function submit() {
  isSubmitted.value = false

  if (!validate()) return

  isLoading.value = true

  // Simulate API call
  await new Promise(resolve => setTimeout(resolve, 1000))

  console.log({
    name: name.value,
    email: email.value,
    message: message.value
  })

  // Reset form
  name.value = ''
  email.value = ''
  message.value = ''

  isLoading.value = false
  isSubmitted.value = true
}
</script>

<template>
  <div class="container">
    <h1>Contact Form</h1>

    <form @submit.prevent="submit">
      
      <!-- Name -->
      <label>Name</label>
      <input v-model="name" type="text" placeholder="Enter your name" />
      <span v-if="errors.name" class="error">{{ errors.name }}</span>

      <!-- Email -->
      <label>Email</label>
      <input v-model="email" type="email" placeholder="Enter your email" />
      <span v-if="errors.email" class="error">{{ errors.email }}</span>

      <!-- Message -->
      <label>Message</label>
      <textarea v-model="message" placeholder="Enter your message"></textarea>
      <span v-if="errors.message" class="error">{{ errors.message }}</span>

      <!-- Button -->
      <button type="submit" :disabled="!isFormValid || isLoading">
        <span v-if="isLoading">Sending...</span>
        <span v-else>Submit</span>
      </button>

      <!-- Success Message -->
      <p v-if="isSubmitted" class="success">
        ✅ Message sent successfully!
      </p>

    </form>
  </div>
</template>

<style scoped>
.container {
  max-width: 400px;
  margin: 40px auto;
  display: flex;
  flex-direction: column;
  font-family: Arial, sans-serif;
}

form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

input, textarea {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 6px;
}

textarea { min-height: 80px; }

button {
  padding: 10px;
  border: none;
  background: black;
  color: white;
  border-radius: 6px;
  cursor: pointer;
}

button:disabled {
  background: gray;
  cursor: not-allowed;
}

.error {
  color: red;
  font-size: 12px;
}

.success {
  color: green;
  margin-top: 10px;
}
</style>