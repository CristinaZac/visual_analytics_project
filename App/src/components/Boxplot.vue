<template>
  <vue-plotly :data="data" :layout="layout" :options="options"/>
</template>

<script>
import VuePlotly from '@statnett/vue-plotly';

export default {
  name: 'Boxplot',
  components: {
    VuePlotly,
  },
  props: {
    bindingBox: Array,
  },
  data() {
    const sewerWater = {
      y: [],
      type: 'box',
      name: 'Sewer and water',
      boxpoints: 'all',
      jitter: 1,
      whiskerwidth: 0.2,
      fillcolor: 'cls',
      marker: {
        size: 2,
        color: 'rgb(0,191,255)',
      },
      line: {
        width: 1,
      },
    };
    const power = {
      y: [],
      type: 'box',
      name: 'Power',
      boxpoints: 'all',
      jitter: 1,
      whiskerwidth: 0.2,
      fillcolor: 'cls',
      marker: {
        size: 2,
        color: 'rgb(255,215,0)',
      },
      line: {
        width: 1,
      },
    };
    const roadsBridges = {
      y: [],
      type: 'box',
      name: 'Roads and bridges',
      boxpoints: 'all',
      jitter: 1,
      whiskerwidth: 0.2,
      fillcolor: 'cls',
      marker: {
        size: 2,
        color: 'rgb(186,85,211)',
      },
      line: {
        width: 1,
      },
    };
    const medical = {
      y: [],
      type: 'box',
      name: 'Medical',
      boxpoints: 'all',
      jitter: 1,
      whiskerwidth: 0.2,
      fillcolor: 'cls',
      marker: {
        size: 2,
        color: 'rgb(60,179,113)',
      },
      line: {
        width: 1,
      },
    };
    const buildings = {
      y: [],
      type: 'box',
      name: 'Buildings',
      boxpoints: 'all',
      jitter: 1,
      whiskerwidth: 0.2,
      fillcolor: 'cls',
      marker: {
        size: 2,
        color: 'rgb(139,69,19)',
      },
      line: {
        width: 1,
      },
    };
    const shakeIntensity = {
      y: [],
      type: 'box',
      name: 'Shake intensity',
      boxpoints: 'all',
      jitter: 1,
      whiskerwidth: 0.2,
      fillcolor: 'cls',
      marker: {
        size: 2,
        color: 'rgb(128,0,0)',
      },
      line: {
        width: 1,
      },
    };
    return {
      data: [sewerWater, power, roadsBridges, medical, buildings, shakeIntensity],
      layout: {
        title: 'Uncertainty in data',
        yaxis: {
          autorange: true,
          showgrid: true,
          zeroline: true,
          dtick: 5,
          gridcolor: 'rgb(255, 255, 255)',
          gridwidth: 1,
          zerolinecolor: 'rgb(255, 255, 255)',
          zerolinewidth: 2,
        },
        margin: {
          l: 40,
          r: 30,
          b: 80,
          t: 100,
        },
        paper_bgcolor: 'rgb(255, 255, 255)',
        plot_bgcolor: 'rgb(255, 255, 255)',
        font: {
          family: 'Avenir, sans-serif',
        },
      },
    };
  },
  watch: {
    bindingBox(item) {
      this.data[0].y = item.map(d => d.sewer_and_water);
      this.data[1].y = item.map(d => d.power);
      this.data[2].y = item.map(d => d.roads_and_bridges);
      this.data[3].y = item.map(d => d.medical);
      this.data[4].y = item.map(d => d.buildings);
      this.data[5].y = item.map(d => d.shake_intensity);
      /*  this.$refs.Plotlychart.$on('click', (data) => {
        for (let i = 0; i < data.points.length; i += 1) {
          this.onchartclickname = data.points[0].x;
        }
      }); */
    },
  },
};

</script>

<style scoped>

</style>
