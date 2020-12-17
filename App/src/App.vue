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
          <b-form-timepicker v-model="time.valuestart" locale="en" minutes-step="5">
          </b-form-timepicker>
          <b-form-timepicker v-model="time.valueend" locale="en" minutes-step="5">
          </b-form-timepicker>
        </b-col>
      </b-row>
      <Boxplot :bindingBox="dataBox"></Boxplot>
          <div>
            <label>Timeline</label>
            <b-form-input id="range-1" v-model="slider.value" type="range" min="0"
                          :max="finalstate"
                          step="5"></b-form-input>
            <div class="mt-2">hours: {{ slider.time }}</div>
          </div>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import crossfilter from 'crossfilter';
// eslint-disable-next-line import/no-extraneous-dependencies
import * as d3 from 'd3';
import Boxplot from './components/Boxplot';

// crossfilter data
let cf;
let dTime; // orario
let dDate; // data
let dlocation;
let dlocation; // quartieri

export default {
  name: 'App',
  components: {
    Boxplot,
  },
  data() {
    return {
      reports: [],
      reportFilter: [],

      finalstate: [],
      slider: {
        time: String,
        value: 0,
      },
      days: {
        options: [],
        value: String,
      },
      time: {
        options: [],
        valuestart: '00:00:00',
        valuestart: '00:05:00',
        valueend: '12:00:00',
      },
      location: {
        selected: String,
        options: [],
      },
      power: {
        value: Number,
        options: [],
        mean: Number,
      },
      buildings: {
        value: Number,
        options: [],
        mean: Number,
      },
      medical: {
        value: Number,
        options: [],
        mean: Number,
      },
      sewer_and_water: {
        value: Number,
        options: [],
        mean: Number,
      },
      roads_and_bridges: {
        value: Number,
        options: [],
        mean: Number,
      },
      shake_intesity: {
        value: Number,
        options: [],
        mean: Number,
      },
      dataBox: [],
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
      if (this.pulsantinotimeseries === true) {
        this.reportFilter = this.reports.filter(value =>
          value.date === this.days.value &&
        value.time === this.slider.time &&
        value.location === this.location.selected);
      } else {
        this.reportFilter = this.reports.filter(value =>
          value.date === this.days.value &&
          value.time >= this.time.valuestart &&
          value.time <= this.time.valueend &&
          value.location === this.location.selected);
      }
      // data manipulation with d3
      this.power.mean = d3.mean(this.reportFilter, d => d.power);
      this.medical.mean = d3.mean(this.reportFilter, d => d.medical);
      this.buildings.mean = d3.mean(this.reportFilter, d => d.buildings);
      this.shake_intesity.mean = d3.mean(this.reportFilter, d => d.shake_intensity);
      this.sewer_and_water.mean = d3.mean(this.reportFilter, d => d.sewer_and_water);
      this.roads_and_bridges.mean = d3.mean(this.reportFilter, d => d.roads_and_bridges);
    functBox() {
      this.dataBox = this.reportFilter.map(v => ({
        power: v.power,
        medical: v.medical,
        buildings: v.buildings,
        sewer_and_water: v.sewer_and_water,
        roads_and_bridges: v.roads_and_bridges,
        shake_intensity: v.shake_intensity,
      }));
    },
    sliderprop() {
      const parts1 = this.time.valuestart.split(':');
      const seconds1 = (parts1[0] * 3600) + (parts1[1] * 60);
      const parts2 = this.time.valueend.split(':');
      const seconds2 = (parts2[0] * 3600) + (parts2[1] * 60);
      this.finalstate = Math.abs(seconds1 - seconds2) / 60;
      const minutes = this.slider.value % 60;
      const hours = Math.floor(this.slider.value / 60) + (parts1[0] * 1);
      if (hours <= 9 && minutes > 9) {
        this.slider.time = `0${hours}:${minutes}:00`;
      } else if (hours > 9 && minutes <= 9) {
        this.slider.time = `${hours}:0${minutes}:00`;
      } else if (hours <= 9 && minutes <= 9) {
        this.slider.time = `0${hours}:0${minutes}:00`;
      } else {
        this.slider.time = `${hours}:${minutes}:00`;
      }
    },
  },
  watch: {
    time: {
      handler(newVal) {
        dTime.filter(newVal.value);
        this.refreshTime();
        this.functBox();
        this.sliderprop();
      },
      deep: true,
    },
    days: {
      handler(newVal) {
        dDate.filter(newVal.value);
        this.refreshTime();
        this.functBox();
        this.sliderprop();
      },
      deep: true,
    },
    location: {
      handler(newVal) {
        dlocation.filter(newVal.value);
        this.refreshTime();
        this.functBox();
        this.sliderprop();
    slider: {
      handler(newVal, oldVal) {
        if (newVal !== oldVal) {
          this.slider.value = newVal;
        }
        this.refreshTime();
        this.functBox();
        this.sliderprop();
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
