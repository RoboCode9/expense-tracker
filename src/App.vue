<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import AddTransaction from './components/AddTransaction.vue';
  import TransactionList from './components/TransactionList.vue';
  import {computed, ref, onMounted} from 'vue'
  import { useToast } from 'vue-toastification'
  const transactions = ref([])
  const toast = useToast()

  // const transactions = ref([
  //   {id: 1, text: 'Paycheck', amount: 777.77},
  //   {id: 2, text: 'Food', amount: -77.77},
  //   {id: 3, text: 'Bills', amount: -577.77},
  //   {id: 4, text: 'Video game', amount: -7.77},
  //   {id: 5, text: 'tax return', amount: 3000},
  // ])

  //get total
  const total = computed(() => {
    return transactions.value.reduce( (acc, transaction) => {
      return acc + transaction.amount
    }, 0)
  })

  //get income by adding all positive values
  const income = computed( () => {
    return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
  })

  //get expenses by adding all negative values
  const expense = computed( () => {
    return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
  })

//handle transactions submitted
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  })
  saveTransactionToLocalStorage()
  toast.success('Transaction Added')
}

//generate unique id
const generateUniqueId = () => {
  return Math.floor(Math.random() * 10000000)
}

//delete transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id)
  saveTransactionToLocalStorage()
  toast.success('Transaction Deleted')
}

//save to local storage
const saveTransactionToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}

//first loads
onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

  if(savedTransactions)
  {
    transactions.value = savedTransactions
  }
})

</script>

<template>
  <Header></Header>
  <div class="container">
    <Balance :total="total"></Balance>
    <IncomeExpenses :income="income" :expense="expense"></IncomeExpenses>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"></TransactionList>

    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"></AddTransaction>
    <!-- {{ transactions }} -->
  </div>
</template>