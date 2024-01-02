<template>
    <h3>Hisrtory</h3>
    <ul id="list" class="list" >
        <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus'">
            {{ transaction.text }} <span>${{ transaction.amount }}</span><button class="delete-btn">x</button>
        </li>
        <!-- <li class="plus">
            Paycheck <span>$800</span><button class="delete-btn">x</button>
        </li> -->
    </ul>
</template>

<script> //options API
export default {
    setup() {
        const transactions = [
            { id: 1, text: 'Flower', amount: -19.99},
            { id: 2, text: 'Salary', amount: 299.97},
            { id: 3, text: 'Book', amount: -10},
            { id: 4, text: 'Camera', amount: 150},
        ];

        const testFunction = () => {
            console.log('test');
        };

        return {
            transactions,
            testFunction,
        };
    }
}
</script>