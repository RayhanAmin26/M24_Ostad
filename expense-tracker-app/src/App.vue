<script setup>
import { ref, watch, onMounted } from 'vue'
import AddTransaction from './components/AddTransaction.vue'
import TransactionList from './components/TransactionList.vue'

const transactions = ref([])

// LocalStorage থেকে লোড করো
onMounted(() => {
  const saved = localStorage.getItem('transactions')
  if (saved) transactions.value = JSON.parse(saved)
})

// LocalStorage এ save করো যখন কিছু চেঞ্জ হয়
watch(transactions, (newVal) => {
  localStorage.setItem('transactions', JSON.stringify(newVal))
}, { deep: true })

// নতুন transaction add করার ফাংশন
function addTransaction(newTransaction) {
  transactions.value.push(newTransaction)
}

// transaction delete করার ফাংশন
function deleteTransaction(index) {
  transactions.value.splice(index, 1)
}
</script>

<template>
  <div class="container mt-4">
    <h2 class="text-center">Expense Tracker</h2>
    <AddTransaction @add="addTransaction" />
    <TransactionList :transactions="transactions" @delete="deleteTransaction" />
  </div>
</template>
