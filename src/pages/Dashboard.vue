<template>
  <div>Daily Sales</div>
  <div id="chart"></div>
</template>

<script lang="ts">
import { onMounted } from 'vue';
import c3 from 'c3';
import axios from 'axios';
export default {
  name: 'Dashboard',
  setup() {
    onMounted(async () => {
      const chart = c3.generate({
        bindto: '#chart',
        data: {
          x: 'x',
          columns: [['x'], ['Sales']],
          types: {
            Sales: 'bar',
          },
        },
        axis: {
          x: {
            type: 'timeseries',
            tick: {
              format: '%Y-%m-%d',
            },
          },
        },
      });

      const { data } = await axios.get('chart');

      chart.load({
        columns: [
          ['x', ...data.map((r: any) => r.date.split('T18')[0])],
          ['Sales', ...data.map((r: any) => r.sum)],
        ],
      });
    });
  },
};
</script>
