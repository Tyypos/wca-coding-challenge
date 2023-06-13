<script setup>
import { defineProps } from 'vue';

defineProps({
    data: Object
});
</script>

<template>
   <div>TODO: chart</div>
   <div v-for="d in data.data">{{ d }}</div>
</template>

<style scoped>
</style>