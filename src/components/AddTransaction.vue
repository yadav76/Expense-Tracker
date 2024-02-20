<template>
  <h3>Add new transaction</h3>
  <form action="" id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input
        type="text"
        name=""
        id="text"
        v-model="text"
        placeholder="Enter Text..."
      />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount<br />
        {negative-expense, positive-income}
      </label>
      <input
        type="text"
        name=""
        id="amount"
        v-model="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add Transaction</button>
  </form>
</template>

<script setup>
import { ref, defineEmits } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();

//now pass the data from child to parent component using "emits"
const emits = defineEmits("submitTransactions");

const text = ref("");
const amount = ref("");

const onSubmit = (event) => {
  event.preventDefault();
  // console.log(text.value, amount.value);
  if (!text.value || !amount.value) {
    toast.error("All fields are required!");
    return;
  }

  //if all fields are filled then add then make an Object of it
  const transactionData = {
    text: text.value,
    amount: amount.value,
  };

  //now pass the data to 'App'
  emits("submitTransactions", transactionData);
  text.value = "";
  amount.value = "";
};
</script>
