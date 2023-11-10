<template>
   <Header />
   <div class="container">
      <Balance :total="+total" />
      <IncomeExpenses :income="+income" :expenses="+expenses"/>
      <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
      <AddTransaction @transactionSubmitted="handleTransactionSubmited"/>
   </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';
  import { useToast } from 'vue-toastification';


   const toast = useToast();
  const transactions = ref([]);

  onMounted(() => {
   const savedTransactions = JSON.parse(localStorage.getItem("transactions"));
   if(savedTransactions){
      transactions.value = savedTransactions;
   }
  });

   const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0);
   });
   //  Get Income
   const income = computed(() => {
    return transactions.value.filter((transaction)=> transaction.amount > 0).reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0).toFixed(2);
   });
   // Get Expenses
   const expenses = computed(() => {
    return transactions.value.filter((transaction)=> transaction.amount < 0).reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0).toFixed(2);
   });

   //Add transaction
   const handleTransactionSubmited = (transactionData) => {
      transactions.value.push({
         id: generateUniqueId(),
         text: transactionData.text,
         amount: transactionData.amount,
      });
      saveTransactionsToLocalStorage();

         toast.success('Transaction added')
   }

   //Generate UniqueID
   const generateUniqueId = () => {
      return Math.floor(Math.random() * 1000000);
   }

   const handleTransactionDeleted = (id) => {
      transactions.value = transactions.value.filter((transaction) => transaction.id !== id );
      saveTransactionsToLocalStorage();

      toast.info('Transaction deleted');
   }
   //Save to localstorage
   const saveTransactionsToLocalStorage = () => {
      localStorage.setItem('transactions', JSON.stringify(transactions.value));
   }
</script>