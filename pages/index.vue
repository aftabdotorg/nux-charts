<script setup lang="ts">
import { Bar } from "vue-chartjs";
import type { ChartData } from "chart.js"
import { computed, ref } from "vue";


type movie = { title: string, rating: number }
const movies = ref<movie[]>([])

fetch("/api.json")
    .then(async (res) => movies.value = await res.json())
    .catch((error) => console.error("Error: ", error.message))

const chartData = computed((): ChartData<"bar"> => {
    return {
        labels: movies.value.map(movie => movie.title),
        datasets: [
            {
                label: "IMDb",
                // backgroundColor: ["#c82834"],
                stack: "rating",
                data: movies.value.map(movie => movie.rating),
            },
            {
                label: "Rotten Tomatoes",
                backgroundColor: ["#244771"],
                stack: "rating",
                data: movies.value.map(movie => movie.rating + 1),
            }
        ]
    }
})


</script>
<template>
    <Bar v-if="movies.length" :data="chartData" :options="{
        indexAxis: 'y'
    }" />
</template>

<style>
body {
    padding: 50px;
}
</style>
