<template>
    <h3>Historique</h3>
    <!-- Container for the list of transactions -->
    <ul id="list" class="list">
        <!-- Iterate through each transaction in the 'transactions' array -->
        <li v-for="transaction in transactions" v-bind:key="transaction.id"
            :class="transaction.amount < 0 ? 'minus' : 'plus'">
            <!-- Display the transaction text -->
            {{ transaction.text }}

            <!-- Display the transaction amount with appropriate styling based on its sign -->
            <span>${{ transaction.amount }}</span>

            <!-- Button for deleting the transaction -->
            <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>



        <!-- 
        <li class="minus">
            Cash <span>-$400</span>
            <button class="delete-btn">x</button>
        </li>
        <li class="plus">
            Paycheck <span>+$800</span>
            <button class="delete-btn">x</button>
        </li>-->
    </ul>
</template>

<script setup>
import { defineProps } from 'vue';
const emit = defineEmits(['transactionDeleted'])
const props = defineProps({
    transactions: {
        type: Array,
        required: true,
    },
})
const deleteTransaction = (id) => {
    emit('transactionDeleted', id)
}
</script>