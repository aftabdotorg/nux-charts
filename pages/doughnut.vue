<script setup lang="ts">
import { Doughnut, Pie } from "vue-chartjs";
import type { ChartData } from "chart.js";
import { computed, ref } from "vue";

const repos = [
    "vuejs/core",
    "facebook/react",
    "angular/angular",
    "sveltejs/svelte",
    "solidjs/solid",
];

const repoData = ref<any[]>([]);

repos.forEach((repo) => {
    fetch(`https://api.github.com/repos/${repo}`).then(async (res) => {
        const repo = await res.json();
        repoData.value.push(repo);
    });
});

const chartData = computed((): ChartData<"pie"> => {
    return {
        labels: repos,
        datasets: [
            {
                backgroundColor: [
                    "#41B883",
                    "#149eca",
                    "#c30e2e",
                    "#f96743",
                    "#043271",
                ],
                label: "Stars",
                data: repoData.value.map((repo) => repo.stargazers_count),
            },
        ],
    };
});
</script>
<template>
    <div style="display: flex; justify-content: center">
        <Pie v-if="repoData.length === repos.length" :data="chartData" :options="{
            cutout: '20%',
            maintainAspectRatio: false
        }" />
    </div>
</template>
<style>
body {
    padding: 50px;
}
</style>