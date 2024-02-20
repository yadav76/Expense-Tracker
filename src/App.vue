<template>
  <HeaderComp />
  <div class="container">
    <BalanceComp :total="total" />
    <IncomeExpenses :income="income" :expenses="expenses" />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="handleDeletedTransaction"
    />
    <AddTransaction @submitTransactions="handleSubmittedTransactions" />
  </div>
</template>

<script setup>
import HeaderComp from "./components/HeaderComp.vue";
import BalanceComp from "./components/BalanceComp.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();

//If there is any transactions saved inside localStorage then get it
onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

const transactions = ref([
  { id: 1, text: "Flower", amount: -19.99 },
  { id: 2, text: "Salary", amount: 299.97 },
  { id: 3, text: "Book", amount: -10 },
  { id: 4, text: "Camera", amount: 150 },
]);

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0);
});
const expenses = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0);
});

//add Data
const handleSubmittedTransactions = (transactionData) => {
  // console.log(transactionData);
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: +transactionData.amount,
  });

  //when new transaction added then save to localStorage
  // saveTransactionsToLocalStorage();

  //transaction added successfully
  toast("Transaction Added Successfully!");
};

//generate UniqueId
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000);
};

//Delete Transaction
const handleDeletedTransaction = (transactionId) => {
  // console.log(transactionId);
  transactions.value = transactions.value.filter((transaction) => {
    return transaction.id != transactionId;
  });

  //when we delete any task then also add to localStorage
  saveTransactionsToLocalStorage();

  toast.success("Transaction Deleted!");
};

//save transactions to localStorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};

// export default {
//   name: "App",
//   components: {
//     HeaderComp,
//     BalanceComp,
//     IncomeExpenses,
//     TransactionList,
//     AddTransaction,
//   },
// };
</script>

<style scoped></style>
