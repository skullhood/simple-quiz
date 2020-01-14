<template>
    <canvas :id="type" :style="{ 'height': height + 'px', 'width': width + 'px'}" :tdot="type">
        
    </canvas>
</template>

<script>
import Chart from 'chart.js';

export default {
  name: 'Chart',
  props: {
      iden: String,
      data: Object,
      type: String,
      width: Number,
      height: Number
  },
  methods: {
        createChart: function() {
            let ctx = document.getElementById(this.type);
            let myChart = new Chart(ctx, {
            type: this.type,
            data: this.data,
            options : {
                legend: {
                    display: false,
                    labels: {
                        fontSize: 20
                    }
                }
            },
                pointLabelFontSize: 20
            });
            return myChart;
        },
    },
  mounted: function(){
      this.createChart()
  }
};
</script>
