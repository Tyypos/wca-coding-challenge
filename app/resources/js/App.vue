<script setup>
import { ref } from 'vue';
import axios from 'axios';
import ChartControl from './components/ChartControl.vue';
import Chart from './components/Chart.vue';

let chartData = ref({});
let isLoaded = ref(false);
const labels = ['HCPCS #11043', 'HCPCS #11044', 'HCPCS #11045', 'HCPCS #11046', 'HCPCS #11047'];
const apiSourceLink = 'https://data.cms.gov/provider-summary-by-type-of-service/medicare-physician-other-practitioners/medicare-physician-other-practitioners-by-geography-and-service';

// get data filtered by state
const filterDataByState = (state) => {
    const url = `https://data.cms.gov/data-api/v1/dataset/6fea9d79-0129-4e4c-b1b8-23cd86a4f435/data-viewer?filter[root-group][group][conjunction]=AND&filter[group-0][group][conjunction]=AND&filter[group-0][group][memberOf]=root-group&filter[filter-0-0][condition][path]=Rndrng_Prvdr_Geo_Lvl&filter[filter-0-0][condition][operator]=%3D&filter[filter-0-0][condition][value]=State&filter[filter-0-0][condition][memberOf]=group-0&filter[filter-0-1][condition][path]=Rndrng_Prvdr_Geo_Desc&filter[filter-0-1][condition][operator]=%3D&filter[filter-0-1][condition][value]=${state}&filter[filter-0-1][condition][memberOf]=group-0&filter[filter-0-2][condition][path]=HCPCS_Cd&filter[filter-0-2][condition][operator]=IN&filter[filter-0-2][condition][value][0]=11043&filter[filter-0-2][condition][value][1]=11044&filter[filter-0-2][condition][value][2]=11045&filter[filter-0-2][condition][value][3]=11046&filter[filter-0-2][condition][value][4]=11047&filter[filter-0-2][condition][memberOf]=group-0`;

    axios.get(url)
        .then(response => {
            formatData(response.data);
        })
        .catch(error => {
            console.error(error);
        });
}

const formatData = (apiData) => {
    const dataSet = getFormattedChartData(getFormattedMedicalData(apiData));

    chartData.value = {
        labels: labels,
        datasets: [
            {
                label: 'Total Services',
                data: dataSet.numServicesDone,
                backgroundColor: ['rgb(54, 162, 235)']
            },
            {
                label: 'Avg. Submitted Charges',
                data: dataSet.submittedCharges,
                backgroundColor: ['rgb(255, 99, 132)']
            },
            {
                label: 'Avg. Medicare Payments',
                data: dataSet.medicarePayments,
                backgroundColor: ['rgb(255, 205, 86)']
            },
        ]
    }

    isLoaded.value = true;
}

// format the data from the api in a way that is easier
// to work with for our chart
const getFormattedMedicalData = (apiData) => {
    const medicalData = {
        11043: {
            oCount: 0,
            fCount: 0,
            averageSubmittedCharges: 0,
            averageMedicarePayments: 0
        },
        11044: {
            oCount: 0,
            fCount: 0,
            averageSubmittedCharges: 0,
            averageMedicarePayments: 0
        },
        11045: {
            oCount: 0,
            fCount: 0,
            averageSubmittedCharges: 0,
            averageMedicarePayments: 0
        },
        11046: {
            oCount: 0,
            fCount: 0,
            averageSubmittedCharges: 0,
            averageMedicarePayments: 0
        },
        11047: {
            oCount: 0,
            fCount: 0,
            averageSubmittedCharges: 0,
            averageMedicarePayments: 0
        }
    }

    // populate medicalData with the values we care about from the api
    // place_of_service (F/O) was broken out for possible future display individually
    apiData.data.forEach(record => {
        if(record[6] === 'F') {
            medicalData[record[3]].fCount = parseInt(record[9]);
        } else {
            medicalData[record[3]].oCount = parseInt(record[9]);
        }
        medicalData[record[3]].averageSubmittedCharges += parseFloat(record[11]);
        medicalData[record[3]].averageMedicarePayments += parseFloat(record[13]);
    });

    return medicalData;
}

// format our data for use by the chart
const getFormattedChartData = (medicalData) => {
    const dataSet = {
        numServicesDone: [],
        submittedCharges: [],
        medicarePayments: []
    }

    for(let key in medicalData) {
        // combine place_of_service (F/O) for simplicity, may show individually in future
        dataSet.numServicesDone.push(medicalData[key].fCount + medicalData[key].oCount);
        dataSet.submittedCharges.push(medicalData[key].averageSubmittedCharges);
        dataSet.medicarePayments.push(medicalData[key].averageMedicarePayments);
    }

    return dataSet;
}
</script>

<template>
   <h1 class="main-header">Medicare Services for Diabetic Foot Ulcer Debridements by State in 2021</h1>
   <p class="data-source">*All data provided by the
        <a :href="apiSourceLink" target="_blank" class="link">CMS API</a>
    </p>
   <ChartControl @filter-change-state="filterDataByState" />
   <Chart :chartData="chartData" :isLoaded="isLoaded" />
</template>

<style scoped>
.main-header {
    text-align: center;
    margin-bottom: 0;
}

.data-source {
    text-align: center;
    font-size: 16px;
}

.link {
    text-decoration: none;
    color: rgb(54, 162, 235);
}
</style>