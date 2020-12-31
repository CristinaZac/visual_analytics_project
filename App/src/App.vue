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
            <h4>Select a location</h4>
            <b-form-select v-model="location.selected" :options="location.options">
            </b-form-select>
          </b-form-group>
        </b-col>
        <b-col>
          <h4>Select a timeframe</h4>
          <b-form-timepicker v-model="time.valuestart" locale="en" minutes-step="5">
          </b-form-timepicker>
          <b-form-timepicker v-model="time.valueend" locale="en" minutes-step="5">
          </b-form-timepicker>
              <div>
                <b-row>
                <b-button block variant="secondary" id="buttonMap"
                          v-b-toggle.collapse-2 class="m-3">
                  Locations map</b-button>
                <b-collapse block variant="secondary" id="collapse-2" class="mb-3">
                 <b-col>
                    <b-card> I am collapsible content!
                      <b-col>
                        <b-card-body>
                            <img alt="Location map image" src="./assets/locationmap.png">
                        </b-card-body>
                      </b-col>
                    </b-card>
                  </b-col>
                </b-collapse>
                </b-row>
              </div>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <b-card
            border-variant="secondary"
            header="Mean of damages"
            header-border-variant="secondary"
            align='left'>
            <b-card-body>
              <b-row>
              <b-col>
              <h5><b-icon icon="lightning"></b-icon> Power:
                {{ power.mean.toPrecision(2) }} </h5>
              <h5><b-icon icon="building"></b-icon> Buildings:
                {{ buildings.mean.toPrecision(2) }} </h5>
              <h5><b-icon icon="suit-heart"></b-icon> Medical:
                {{ medical.mean.toPrecision(2) }} </h5>
              <h5><b-icon icon="droplet"></b-icon> Sewer and water:
                {{ sewer_and_water.mean.toPrecision(2) }} </h5>
              <h5><b-icon icon="cone-striped"></b-icon> Roads and bridges:
                {{ roads_and_bridges.mean.toPrecision(2) }} </h5>
              <h5><b-icon icon="bullseye"></b-icon> Shake intensity:
                {{ shake_intensity.mean.toPrecision(2) }} </h5>
              </b-col>
              <b-col>
              <Piechart :bindingPie="dataPie"></Piechart>
              </b-col>
              </b-row>
            </b-card-body>
            <b-card-footer>
              <h6>Number of reports: {{ nReports }} </h6>
            </b-card-footer>
          </b-card>
        </b-col>
      </b-row>
      <Stackedbar :bindingStacked="dataStacked"></Stackedbar>
      <Boxplot :bindingBox="dataBox"></Boxplot>
      <b-row>
        <div>
          <b-button :pressed.sync="myButtonTimeline" variant="secondary"
                    @click="disable = !disable">
            {{ disable ? 'Disable' : 'Enable' }} timeline
          </b-button>
          <p> <strong>{{ myButtonTimeline }}</strong></p>
        </div>
      </b-row>
      <b-row>
        <b-col>
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
import Piechart from './components/Piechart';
import Stackedbar from './components/Stackedbar';

// crossfilter data
let cf;
let dTime; // orario
let dDate; // data
let dlocation; // quartieri
// TODO aggiungere mappa
export default {
  name: 'App',
  components: {
    Boxplot,
    Piechart,
    Stackedbar,
  },
  data() {
    return {
      reports: [],
      reportFilter: [],
      reportStackedFilter: [],
      finalstate: [],
      buttonTimeline: false,
      disable: false,
      nReports: 0,
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
        mean_i: [],
      },
      buildings: {
        value: Number,
        options: [],
        mean: Number,
        mean_i: [],
      },
      medical: {
        value: Number,
        options: [],
        mean: Number,
        mean_i: [],
      },
      sewer_and_water: {
        value: Number,
        options: [],
        mean: Number,
        mean_i: [],
      },
      roads_and_bridges: {
        value: Number,
        options: [],
        mean: Number,
        mean_i: [],
      },
      shake_intensity: {
        value: Number,
        options: [],
        mean: Number,
        mean_i: [],
      },
      dataBox: [],
      dataPie: [],
      dataStacked: [],
      myButtonTimeline: false,
      buttons: [{
        caption: 'Abilitate timeline',
        state: true },
      ],
      computed: {
        btnStates() {
          return this.buttons.map(btn => btn.state);
        },
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
        this.days.options = ['All'].concat(dDate
          .group().reduceCount().all().map(v => v.key));
        this.location.options = dlocation
          .group().reduceCount().all().map(v => v.key);
        this.location.selected = this.location.options[0];
        this.days.value = this.days.options[0];
      });
  },

  methods: {
    refreshTime() {
      if (this.myButtonTimeline === true && this.days.value !== 'All') {
        this.reportFilter = this.reports.filter(value =>
          value.date === this.days.value &&
        value.time === this.slider.time &&
        value.location === this.location.selected);
      } else if (this.myButtonTimeline === true && this.days.value === 'All') {
        this.reportFilter = this.reports.filter(value =>
          value.time === this.slider.time &&
        value.location === this.location.selected);
      } else if (this.days.value === 'All') {
        this.reportFilter = this.reports.filter(value =>
          value.time >= this.time.valuestart &&
        value.time <= this.time.valueend &&
          value.location === this.location.selected);
      } else {
        this.reportFilter = this.reports.filter(value =>
          value.date === this.days.value &&
          value.time >= this.time.valuestart &&
          value.time <= this.time.valueend &&
          value.location === this.location.selected);
      }
      // data manipulation with d3
      this.nReports = this.reportFilter.length;
      this.power.mean = d3.mean(this.reportFilter, d => d.power);
      this.medical.mean = d3.mean(this.reportFilter, d => d.medical);
      this.buildings.mean = d3.mean(this.reportFilter, d => d.buildings);
      this.shake_intensity.mean = d3.mean(this.reportFilter, d => d.shake_intensity);
      this.sewer_and_water.mean = d3.mean(this.reportFilter, d => d.sewer_and_water);
      this.roads_and_bridges.mean = d3.mean(this.reportFilter, d => d.roads_and_bridges);
      this.dataPie = [
        this.power.mean,
        this.medical.mean,
        this.buildings.mean,
        this.shake_intensity.mean,
        this.sewer_and_water.mean,
        this.roads_and_bridges.mean];
    },
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
    //  functPie() {
    /* this.dataPie = this.reportFilter.map(v => ({
        power: v.power,
        medical: v.medical,
        buildings: v.buildings,
        sewer_and_water: v.sewer_and_water,
        roads_and_bridges: v.roads_and_bridges,
        shake_intensity: v.shake_intensity,
      })); */
    // [
    // this.power.mean,
    // this.medical.mean,
    // ];
    // },

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
    /* selector() {
      const all = this.reports;
      if (this.days.value = all) {
        // TODO opzioni all
      }
    }, */
    refreshStacked() {
      if (this.myButtonTimeline === true && this.days.value !== 'All') {
        this.reportStackedFilter = this.reports.filter(value =>
          value.date === this.days.value &&
          value.time === this.slider.time);
      } else if (this.myButtonTimeline === true && this.days.value === 'All') {
        this.reportStackedFilter = this.reports.filter(value =>
          value.time === this.slider.time);
      } else if (this.days.value === 'All') {
        this.reportStackedFilter = this.reports.filter(value =>
          value.time >= this.time.valuestart &&
          value.time <= this.time.valueend);
      } else {
        this.reportStackedFilter = this.reports.filter(value =>
          value.date === this.days.value &&
          value.time >= this.time.valuestart &&
          value.time <= this.time.valueend);
      }
      // data manipulation with d3
      this.power.mean_i = [];
      for (let i = 1; i < 20; i += 1) {
        this.power.mean_i.push(d3.mean(this.reportStackedFilter
          .filter(value => value.location === i), d => d.power));
      }
      this.medical.mean_i = [];
      for (let i = 1; i < 20; i += 1) {
        this.medical.mean_i.push(d3.mean(this.reportStackedFilter
          .filter(value => value.location === i), d => d.medical));
      }
      this.sewer_and_water.mean_i = [];
      for (let i = 1; i < 20; i += 1) {
        this.sewer_and_water.mean_i.push(d3.mean(this.reportStackedFilter
          .filter(value => value.location === i), d => d.sewer_and_water));
      }
      this.shake_intensity.mean_i = [];
      for (let i = 1; i < 20; i += 1) {
        this.shake_intensity.mean_i.push(d3.mean(this.reportStackedFilter
          .filter(value => value.location === i), d => d.shake_intensity));
      }
      this.buildings.mean_i = [];
      for (let i = 1; i < 20; i += 1) {
        this.buildings.mean_i.push(d3.mean(this.reportStackedFilter
          .filter(value => value.location === i), d => d.buildings));
      }
      this.roads_and_bridges.mean_i = [];
      for (let i = 1; i < 20; i += 1) {
        this.roads_and_bridges.mean_i.push(d3.mean(this.reportStackedFilter
          .filter(value => value.location === i), d => d.roads_and_bridges));
      }
    },
    functStacked() {
      this.dataStacked = [
        this.power.mean_i,
        this.medical.mean_i,
        this.buildings.mean_i,
        this.shake_intensity.mean_i,
        this.sewer_and_water.mean_i,
        this.roads_and_bridges.mean_i];
    },
  },
  watch: {
    time: {
      handler(newVal) {
        dTime.filter(newVal.value);
        this.refreshTime();
        this.functBox();
        // this.functPie();
        this.sliderprop();
        this.refreshStacked();
        this.functStacked();
      },
      deep: true,
    },
    days: {
      handler(newVal) {
        dDate.filter(newVal.value);
        this.refreshTime();
        this.functBox();
        // this.functPie();
        this.sliderprop();
        this.refreshStacked();
        this.functStacked();
      },
      deep: true,
    },
    location: {
      handler(newVal) {
        dlocation.filter(newVal.value);
        this.refreshTime();
        this.functBox();
        // this.functPie();
        this.sliderprop();
      },
      deep: true,
    },
    slider: {
      handler(newVal, oldVal) {
        if (newVal !== oldVal) {
          this.slider.value = newVal;
        }
        this.refreshTime();
        this.functBox();
        // this.functPie();
        this.sliderprop();
        this.refreshStacked();
        this.functStacked();
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
