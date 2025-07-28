<script setup>
import { ref } from 'vue'

const title = ref('')
const amount = ref(null)
const type = ref('Income')

const emit = defineEmits(['add'])
const error = ref('')

function add() {
  if (!title.value || !amount.value || amount.value <= 0) {
    error.value = 'Please enter valid title and amount.'
    return
  }

  emit('add', {
    title: title.value,
    amount: parseFloat(amount.value),
    type: type.value.toUpperCase(),
  })

  title.value = ''
  amount.value = null
  type.value = 'Income'
  error.value = ''
}
</script>

<template>
  <div class="card my-3 p-3">
    <h5>Add Transaction</h5>
    <div v-if="error" class="text-danger">{{ error }}</div>

    <input v-model="title" type="text" placeholder="Title" class="form-control my-1" />
    <input v-model="amount" type="number" placeholder="Amount" class="form-control my-1" />
    <select v-model="type" class="form-control my-1">
      <option>Income</option>
      <option>Expense</option>
    </select>
    <button @click="add" class="btn btn-primary mt-2">Add</button>
  </div>
</template>
