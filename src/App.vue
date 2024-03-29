<template>
  <Header />
  <div class="container">
    <Balance :total="total"/>
    <IncomeExpenses :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import {useToast} from 'vue-toastification';

import { ref, computed, onMounted } from 'vue';
// onMounted is a life cycle method which fires off automatically when the component mounts

const toast = useToast();

// const transactions = ref([
//     { id: 1, text: 'Flower', amount: -19.99},
//     { id: 2, text: 'Salary', amount: 299.97},
//     { id: 3, text: 'Book', amount: -10},
//     { id: 4, text: 'Camera', amount: 150},
// ]); // by wrapping the array with ref, transactions will be reactive
const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if(savedTransactions) {
    transactions.value = savedTransactions;
  }
});
// console.log(transactions.value);

// Get total
const total = computed(() => {
  return transactions.value.reduce((accumulator, transaction) => {
    return accumulator + transaction.amount;
  }, 0); //start with 0(acc value) and it'll loop through and add all transaction.amount
})

// Get income
const income = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount > 0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0).toFixed(2);
})

// Get expenses
const expenses = computed(() => {
  return transactions.value
  .filter((transaction) => transaction.amount < 0)
  .reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0).toFixed(2);
})

// Add transaction
const handleTransactionSubmitted = (transactionData) => {
  // console.log(transactionData);
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount
  })

  saveTransactionsToLocalStorage();

  toast.success('Transaction added');
}

// Generate unique ID
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
}

// Delete transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id);

  saveTransactionsToLocalStorage();

  toast.success('Transaction deleted');
}

// Save to localstorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}
</script>