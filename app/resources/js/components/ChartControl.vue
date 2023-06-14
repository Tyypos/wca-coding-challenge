<script setup>
import { defineEmits } from 'vue';

const emit = defineEmits(['filter-change-state']);
const states = ['Alabama','Alaska','Arizona','Arkansas','California','Colorado','Connecticut','Delaware','Florida','Georgia','Hawaii','Idaho','Illinois','Indiana','Iowa','Kansas','Kentucky','Louisiana','Maine','Maryland','Massachusetts','Michigan','Minnesota','Mississippi','Missouri','Montana','Nebraska','Nevada','New Hampshire','New Jersey','New Mexico','New York','North Carolina','North Dakota','Ohio','Oklahoma','Oregon','Pennsylvania','Rhode Island','South Carolina','South Dakota','Tennessee','Texas','Utah','Vermont','Virginia','Washington','West Virginia','Wisconsin','Wyoming'];

const changeState = (event) => {
    emit('filter-change-state', event.target.value);
}
</script>

<template>
   <form class="form">
        <p class="filter-header">Filter By</p>
        <fieldset class="filter-container">
            <label for="state" class="filter-label">State</label>
            <select id="state" name="state" @change="changeState">
                <option value="" selected disabled>-</option>
                <option v-for="state in states" :value="state">{{ state }}</option>
            </select>
        </fieldset>
    </form>
</template>

<style scoped>
.form {
    padding: 30px 30px 40px 30px;
    background: #ededed;
    max-width: 1044px;
    margin: auto;
}

.filter-header {
    margin: 0;
    padding: 10px 0 20px 0;
    font-weight: 600;
    font-size: 20px;
}

.filter-container {
    border: none;
    padding: 0;
    margin: 0;
}

.filter-label {
    margin-right: 4px;
}
</style>