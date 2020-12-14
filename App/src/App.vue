<template>
  <div id="app">
    <b-container class="bv-example-row">
      <b-row>
        <b-col>
          <b-form-group>
            <h4>Select a date</h4>
            <b-form-checkbox-group
              v-model="days.value"
              :options="days.options"
              name="buttons-1"
              buttons
            ></b-form-checkbox-group>
          </b-form-group>
          <b-form-group>
            <h4>Select a neighborhood</h4>
            <b-form-select v-model="location.selected" :options="location.options"></b-form-select>
          </b-form-group>
        </b-col>
        <b-col>
          <h4>Select a timeframe</h4>
              <b-form-timepicker v-model="time.valuestart" locale="en"></b-form-timepicker>
              <b-form-timepicker v-model="time.valueend" locale="en"></b-form-timepicker>
        </b-col>
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
let dlocation;

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      reports: [],
      reportFilter: [],

      days: {
        options: [],
        value: String,
      },
      time: {
        options: [],
        valuestart: '00:00:00',
        valueend: '12:00:00',
      },
      location: {
        selected: String,
        options: [],
      },
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
        cf = crossfilter(this.reports);
        dDate = cf.dimension(d => d.date);
        dTime = cf.dimension(d => d.time);
        // eslint-disable-next-line camelcase
        dlocation = cf.dimension(d => d.location);
        dDate.group().reduceCount().all();
        dTime.group().reduceCount().all();
        dlocation.group().reduceCount().all();

        // this.time.options = dDate_h.group().reduceCount().all().map(v => v.key);
        // this.time.value = this.time.options[0];
        this.days.options = dDate
          .group().reduceCount().all().map(v => v.key);
        this.days.value = this.days.options[0];
        this.location.options = dlocation
          .group().reduceCount().all().map(v => v.key);
        this.location.selected = this.location.options[0];
      });
  },

  methods: {
    refreshTime() {
      this.reportFilter = this.reports.filter(value =>
        value.date === this.days.value &&
        value.time >= this.time.valuestart &&
        value.time <= this.time.valueend &&
        value.location === this.location.selected);
    },
  },
  watch: {
    time: {
      handler(newVal) {
        dTime.filter(newVal.value);
        this.refreshTime();
      },
      deep: true,
    },
    days: {
      handler(newVal) {
        dDate.filter(newVal.value);
        this.refreshTime();
      },
      deep: true,
    },
    location: {
      handler(newVal) {
        dlocation.filter(newVal.value);
        this.refreshTime();
      },
      deep: true,
    },
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
