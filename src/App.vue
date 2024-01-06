<template>
    <Header />
    <div class="container">
        <Balance :total="+total" />
        <IncomeExpensive :income="+income" :expenses="+expenses" />
        <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
        <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    </div>
</template>


<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpensive from './components/IncomeExpensive.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from "./components/AddTransaction.vue";
import { useToast } from 'vue-toastification';
const toast = useToast();
import { ref, computed, onMounted } from 'vue';

const transactions = ref([]);

onMounted(() => {
    const savedTransaction = JSON.parse(localStorage.getItem("transactions"));
    if (savedTransaction) {
        transactions.value = savedTransaction;
    }
});
console.log(transactions)
//get total
const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0);
})

// get income

const income = computed(() => {
    return transactions.value.filter((transaction) => transaction.amount > 0).reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0).toFixed(2);
})



//get expenses
const expenses = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0).toFixed(2);
})

//add transaction

const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
        id: generateUniqueId(),
        text: transactionData.text,
        amount: transactionData.amount
    });
    saveTransactionToLocasStorage()
    console.log(generateUniqueId());
    toast.success("Transaction added");
};

const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000);

}

// delete transaction
const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
    saveTransactionToLocasStorage()

    toast.success('Transaction deleted');
};

//save to localstorage
const saveTransactionToLocasStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
}


</script>

