<template>
    <div class="h-auto max-w-[90vh] w-full flex flex-col">
        <canvas class="w-full h-full m-auto" ref="myChart"></canvas>
        <v-switch v-model="enableAnimation" :label="`Animation${enableAnimation ? '✨' : ''}`" class="ml-4"
            :color="enableAnimation ? 'orange' : ''" hide-details inset></v-switch>
    </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'
import { Chart as ChartJS } from 'chart.js/auto';
import { getChartLabelPlugin } from 'chart.js-plugin-labels-dv'
ChartJS.register(getChartLabelPlugin())
import 'chartjs-adapter-date-fns';


let chart;
const myChart = ref(null);

const enableAnimation = ref(false)

onMounted(() => {
    const ctx = myChart.value.getContext('2d');
    chart = new ChartJS(
        ctx, {
            type: 'doughnut'
    });

    chart.data = { labels: [], datasets: [{ data: [] }] };
    chart.options = options;
    chart.update();
});

watch(enableAnimation, (value) => {
    toggleAnimation(value);
})

const options = {
    animation: {
        animateRotate: false,
        animateScale: false
    },
        plugins: {
            labels: {
                precision: 1,
                fontSize: Math.min(window.innerHeight / 25, window.innerWidth/ 25),
                fontColor: '#fff',
                fontStyle: 'bold',
                textShadow: true,
                render: function (args) {
                    return args.percentage > 4 ? args.percentage + "%":''
                }
            },
            legend: {
                maxHeight: Math.min(window.innerHeight /5, window.innerWidth / 5),
            }
    }
}
function setData(data) {
    chart.data = data
    chart.update()
}

function addData(dataset) {
    // TODO
}

function removeData(label) {
    // TODO
}

function toggleAnimation(value) {
    chart.options.animation.animateRotate = value;
    chart.options.animation.animateScale = value;
}

defineExpose({ addData, removeData, setData })

</script>