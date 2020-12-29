<template>
  <vue-plotly :data="data" :layout="layout"/>
</template>

<script>
import VuePlotly from '@statnett/vue-plotly';

export default {
  name: 'Stackedbar',
  components: {
    VuePlotly,
  },
  data() {
    const trace1 = {
      x: ['giraffes', 'orangutans', 'monkeys'],
      y: [20, 14, 23],
      name: 'SF Zoo',
      type: 'bar',
    };
    const trace2 = {
      x: ['giraffes', 'orangutans', 'monkeys'],
      y: [12, 18, 29],
      name: 'LA Zoo',
      type: 'bar',
    };
    return {
      data: [trace1, trace2],
      layout: {
        barmode: 'stack',
      },
    };
  },
};
</script>

<style scoped>

</style>
