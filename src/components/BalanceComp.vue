<template>
  <h4>Your Balance</h4>
  <div class="balance-container">
    <h1 id="balance">${{ props.total }}</h1>
    <button @click="transactionsReset" class="reset-btn">Reset</button>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";

const emit = defineEmits("reset");

const transactionsReset = () => {
  emit("reset", true);
};

const props = defineProps({
  total: {
    type: Number,
    required: true,
  },
});
</script>

<style scoped>
.balance-container {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
}
.reset-btn {
  background-color: #ffffff; /* Background color */
  color: #333333; /* Text color */
  border: 1px solid #cccccc; /* Border */
  padding: 8px 16px; /* Padding */
  font-size: 16px; /* Font size */
  border-radius: 4px; /* Border radius */
  cursor: pointer; /* Cursor style */
  transition: background-color 0.3s, color 0.3s, border-color 0.3s; /* Transition effect */
}

/* Hover state */
.reset-btn:hover {
  background-color: #f0f0f0; /* Background color on hover */
  color: #555555; /* Text color on hover */
}

/* Active state */
.reset-btn:active {
  background-color: #e0e0e0; /* Background color when clicked */
  border-color: #aaaaaa; /* Border color when clicked */
}
</style>
