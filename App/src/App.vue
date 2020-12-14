<template>
  <div id="app">
    <b-container class="bv-example-row">
      <b-row>
        <b-col>1 of 3</b-col>
        <b-col>2 of 3</b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import crossfilter from 'crossfilter';

// crossfilter data
let cf;
let dTime; // orario
let dDate; // data
// eslint-disable-next-line camelcase
let dDate_h; // data e ora
let dlocation;

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      reports: [],
    };
  },
  mounted() {
    fetch('static/mc1_t.json')
      .then(res => res.json())
      .then((out) => {
        this.reports = out.map(d => ({
          time: d.time,
          date: d.date,
          date_h: d.date_h,
          location: d.location,
          sewer_and_water: d.sewer_and_water,
          power: d.power,
          roads_and_bridges: d.roads_and_bridges,
          medical: d.medical,
          buildings: d.buildings,
          shake_intensity: d.shake_intensity,

        }));
      });
    cf = crossfilter(this.reports);
    dDate = cf.dimension(d => d.date);
    dTime = cf.dimension(d => d.time);
    // eslint-disable-next-line camelcase
    dDate_h = cf.dimension(d => d.date_h);
    dlocation = cf.dimension(d => d.location);
    /* eslint-disable */
    console.log('time', dTime.group().reduceCount().all());
    console.log('date', dDate.group().reduceCount().all());
    console.log('date_h', dDate_h.group().reduceCount().all());
    console.log('location', dlocation.group().reduceCount().all());

   // this.date.options = dDate.group().reduceCount().all().map(v => v.key);
   // this.date.value = this.date.options[0];
  },
};

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
