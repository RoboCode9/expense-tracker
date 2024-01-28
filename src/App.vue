<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import {computed, ref} from 'vue'

  const transactions = ref([
    {id: 1, text: 'Paycheck', amount: 777.77},
    {id: 2, text: 'Food', amount: -77.77},
    {id: 3, text: 'Bills', amount: -577.77},
    {id: 4, text: 'Video game', amount: -7.77},
    {id: 5, text: 'tax return', amount: 3000},
  ])

  //get total
  const total = computed(() => {
    return transactions.value.reduce( (acc, transaction) => {
      return acc + transaction.amount
    }, 0)
  })
</script>

<template>
  <Header></Header>
  <div class="container">
    <Balance :total="total"></Balance>
  </div>
</template>