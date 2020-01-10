<template>
    <canvas id="chart"></canvas>
</template>

<script>
import Chart from 'chart.js';

export default {
  name: 'Chart',
  props: {
      data: Object,
      type: String,
      width: Number,
      height: Number
  },
  methods: {
    createChart: function() {
        const ctx = document.getElementById('chart');
        const myChart = new Chart(ctx, {
        type: this.type,
        data: this.data,
        options : {}
        });
    },
    }
};
</script>
