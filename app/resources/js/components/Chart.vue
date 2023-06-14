<script setup>
import { defineProps } from 'vue';
import { Bar } from 'vue-chartjs';
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js';

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale);

defineProps({
    chartData: Object,
    isLoaded: Boolean
});
</script>

<template>
    <div class="chart-container" v-if="isLoaded">
        <Bar :data="chartData" />
    </div>
</template>

<style scoped>
.chart-container {
    border: 1px solid black;
    padding: 50px;
    text-align: center;
    max-width: 1000px;
    margin: 20px auto;
}
</style>