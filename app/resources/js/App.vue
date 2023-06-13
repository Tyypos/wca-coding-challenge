<script setup>
import { ref } from 'vue';
import axios from 'axios';
import ChartControl from './components/ChartControl.vue';
import Chart from './components/Chart.vue';

let data = ref({});

const filterDataByState = (state) => {
    const url = `https://data.cms.gov/data-api/v1/dataset/6fea9d79-0129-4e4c-b1b8-23cd86a4f435/data-viewer?filter[root-group][group][conjunction]=AND&filter[group-0][group][conjunction]=AND&filter[group-0][group][memberOf]=root-group&filter[filter-0-0][condition][path]=Rndrng_Prvdr_Geo_Lvl&filter[filter-0-0][condition][operator]=%3D&filter[filter-0-0][condition][value]=State&filter[filter-0-0][condition][memberOf]=group-0&filter[filter-0-1][condition][path]=Rndrng_Prvdr_Geo_Desc&filter[filter-0-1][condition][operator]=%3D&filter[filter-0-1][condition][value]=${state}&filter[filter-0-1][condition][memberOf]=group-0&filter[filter-0-2][condition][path]=HCPCS_Cd&filter[filter-0-2][condition][operator]=IN&filter[filter-0-2][condition][value][0]=11043&filter[filter-0-2][condition][value][1]=11044&filter[filter-0-2][condition][value][2]=11045&filter[filter-0-2][condition][value][3]=11046&filter[filter-0-2][condition][value][4]=11047&filter[filter-0-2][condition][memberOf]=group-0`;

    axios.get(url)
        .then(reponse => {
            data.value = reponse.data;
            console.log('data', reponse.data);
        })
        .catch(error => {
            console.error(error);
        });
}
</script>

<template>
   <h1 class="main-header">Medicare Services for Diabetic Foot Ulcer Debridements by State in 2021</h1>
   <ChartControl @filter-change-state="filterDataByState" />
   <Chart :data="data" />
</template>

<style scoped>
.main-header {
    text-align: center;
}
</style>