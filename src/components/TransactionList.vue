<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactions"
        :key="transaction.id"
        :class="transaction.amount < 0 ? 'minus' : 'plus'">
            {{ transaction.text }}
            <button class="delete-btn">X</button>
        </li>
    </ul>
</template>
<script setup>
    const transactions = [
        { id: 1 , text: 'Flower', amount: -19.99},
        { id: 2 , text: 'Salary', amount: 299.97},
    ]

</script>