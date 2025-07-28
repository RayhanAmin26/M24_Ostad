<script setup>
import { ref, computed } from 'vue'

const props = defineProps(['transactions'])
const emit = defineEmits(['delete'])

const filter = ref('all')

const filteredTransactions = computed(() => {
  if (filter.value === 'income') {
    return props.transactions.filter(t => t.type === 'INCOME')
  } else if (filter.value === 'expense') {
    return props.transactions.filter(t => t.type === 'EXPENSE')
  } else {
    return props.transactions
  }
})

function deleteTransaction(index) {
  emit('delete', index)
}
</script>

<template>
  <div>
    <div class="btn-group mb-2">
      <button class="btn btn-outline-primary" @click="filter = 'all'">All</button>
      <button class="btn btn-outline-success" @click="filter = 'income'">Income</button>
      <button class="btn btn-outline-danger" @click="filter = 'expense'">Expense</button>
    </div>

    <div v-if="filteredTransactions.length === 0">
      No transactions recorded yet.
    </div>

    <table v-else class="table table-bordered">
      <thead>
        <tr>
          <th>Title</th>
          <th>Amount</th>
          <th>Type</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(t, index) in filteredTransactions" :key="index"
            :class="{'fw-bold': t.amount >= 500}">
          <td>{{ t.title }}</td>
          <td :class="t.type === 'INCOME' ? 'text-success' : 'text-danger'">${{ t.amount }}</td>
          <td>{{ t.type }}</td>
          <td><button @click="deleteTransaction(index)" class="btn btn-sm btn-danger">Delete</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
