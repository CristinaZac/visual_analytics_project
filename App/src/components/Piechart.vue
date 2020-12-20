<template>
  <vue-plotly :data="data" :layout="layout"/>
</template>

<script>
// eslint-disable-next-line import/no-extraneous-dependencies
import VuePlotly from '@statnett/vue-plotly';

export default {
  name: 'Piechart',
  components: {
    VuePlotly,
  },
  props: {
    bindingPie: Array,
  },
  data() {
    const data = {
      values: [],
      labels: ['Power', 'Medical', 'Buildings', 'Shake intensity',
        'Sewer and water', 'Roads and bridges'],
      type: 'pie',
      domain: {
        x: [0, 0.85],
      },
      marker: {
        colors: ['rgb(255,215,0)', 'rgb(60,179,113)', 'rgb(167, 93, 34)',
          'rgb(244,0,0)', 'rgb(0,191,255)', 'rgb(186,85,211)'],
      },
    };
    return {
      data: [data],
      layout: {
        height: 400,
        width: 550,
        font: {
          family: 'Avenir, sans-serif',
        },
      },
    };
  },
  watch: {
    bindingPie(datum) {
      this.data[0].values = datum;
    },
    deep: true,
    immediate: true,
  },
};

</script>

<style scoped>

</style>
