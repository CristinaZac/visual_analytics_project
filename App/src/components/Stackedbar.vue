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
  props: {
    bindingStacked: Array,
  },
  data() {
    const power = {
      x: ['Location 1', 'Location 2', 'Location 3', 'Location 4', 'Location 5', 'Location 6', 'Location 7',
        'Location 8', 'Location 9', 'Location 10', 'Location 11', 'Location 12', 'Location 13',
        'Location 14', 'Location 15', 'Location 16', 'Location 17', 'Location 18', 'Location 19'],
      y: [],
      name: 'Power',
      type: 'bar',
      marker: {
        color: 'rgb(255,215,0)',
      },
    };
    const medical = {
      x: ['Location 1', 'Location 2', 'Location 3', 'Location 4', 'Location 5', 'Location 6', 'Location 7',
        'Location 8', 'Location 9', 'Location 10', 'Location 11', 'Location 12', 'Location 13',
        'Location 14', 'Location 15', 'Location 16', 'Location 17', 'Location 18', 'Location 19'],
      y: [],
      name: 'Medical',
      type: 'bar',
      marker: {
        color: 'rgb(60,179,113)',
      },
    };
    // eslint-disable-next-line camelcase
    const roads_and_bridges = {
      x: ['Location 1', 'Location 2', 'Location 3', 'Location 4', 'Location 5', 'Location 6', 'Location 7',
        'Location 8', 'Location 9', 'Location 10', 'Location 11', 'Location 12', 'Location 13',
        'Location 14', 'Location 15', 'Location 16', 'Location 17', 'Location 18', 'Location 19'],
      y: [],
      name: 'Roads and bridges',
      type: 'bar',
      marker: {
        color: 'rgb(186,85,211)',
      },
    };
    // eslint-disable-next-line camelcase
    const sewer_and_water = {
      x: ['Location 1', 'Location 2', 'Location 3', 'Location 4', 'Location 5', 'Location 6', 'Location 7',
        'Location 8', 'Location 9', 'Location 10', 'Location 11', 'Location 12', 'Location 13',
        'Location 14', 'Location 15', 'Location 16', 'Location 17', 'Location 18', 'Location 19'],
      y: [],
      name: 'Sewer and water',
      type: 'bar',
      marker: {
        color: 'rgb(0,191,255)',
      },
    };
    const buildings = {
      x: ['Location 1', 'Location 2', 'Location 3', 'Location 4', 'Location 5', 'Location 6', 'Location 7',
        'Location 8', 'Location 9', 'Location 10', 'Location 11', 'Location 12', 'Location 13',
        'Location 14', 'Location 15', 'Location 16', 'Location 17', 'Location 18', 'Location 19'],
      y: [],
      name: 'Buildings',
      type: 'bar',
      marker: {
        color: 'rgb(167, 93, 34)',
      },
    };
    // eslint-disable-next-line camelcase
    const shake_intensity = {
      x: ['Location 1', 'Location 2', 'Location 3', 'Location 4', 'Location 5', 'Location 6', 'Location 7',
        'Location 8', 'Location 9', 'Location 10', 'Location 11', 'Location 12', 'Location 13',
        'Location 14', 'Location 15', 'Location 16', 'Location 17', 'Location 18', 'Location 19'],
      y: [],
      name: 'Shake intensity',
      type: 'bar',
      marker: {
        color: 'rgb(244,0,0)',
      },
    };
    return {
      // eslint-disable-next-line camelcase
      data: [power, medical, buildings, shake_intensity, sewer_and_water, roads_and_bridges],
      layout: {
        barmode: 'stack',
        font: {
          family: 'Avenir, sans-serif',
        },
      },
    };
  },
  watch: {
    bindingStacked(item) {
      this.data[0].y = item[0];
      this.data[1].y = item[1];
      this.data[2].y = item[2];
      this.data[3].y = item[3];
      this.data[4].y = item[4];
      this.data[5].y = item[5];
    },
  },
};
</script>

<style scoped>

</style>
