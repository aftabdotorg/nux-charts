<script setup lang="ts">
import { Line } from "vue-chartjs";
import type { ChartData } from "chart.js"
import { computed, ref } from "vue";

const weatherData = ref()

fetch("https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&current=temperature_2m,wind_speed_10m&hourly=temperature_2m,relative_humidity_2m,wind_speed_10m")
    .then(async (res) => weatherData.value = await res.json())
    .catch((error) => console.error("Error: ", error.message))

const chartData = computed((): ChartData<"line"> => {
    return {
        labels: weatherData.value.hourly.time.map((timestamp: string) => useDateFormat(timestamp, "ddd h aa").value),
        datasets: [
            {
                label: "Temperature",
                backgroundColor: ["#c82834"],
                data: weatherData.value.hourly.temperature_2m,
            }
        ]
    }
})


</script>
<template>
    <Line v-if="weatherData" :data="chartData" />
</template>

<style>
body {
    padding: 50px;
}
</style>
