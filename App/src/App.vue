<template>
  <div id="app">
    <h3 class="mb-3">Mini-Challenge 1</h3>
    <h2 class="mb-4">Crowdsourcing for Situational Awareness</h2>
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
              button-variant="dark"
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
                  <b-row class="w-100">
                  <b-button-group class="mt-3 mb-3 ml-3 pl-3 w-100">
                    <b-button variant="dark" class="pl-5 pr-5 w-100" v-b-toggle.sidebar-1>
                      Data analysis</b-button>
                    <b-button variant="dark" class="pr-5 pl-5 w-100" id="buttonMap"
                              v-b-toggle.collapse-2> St. Himark map</b-button>
                  </b-button-group>
                  </b-row>
                  <div class="p-0 bd-highlight">
                    <b-sidebar id="sidebar-1" title="Data analysis" shadow>
                      <div class="px-3 py-2">
                        <p class="text-justify">
                          This web app fulfills the purpose of representing and analyzing
                          the data of the Vast Mini Challenge 1 in order to answer to the issues
                          presented by the challenge. <br>
                          The information analyzed are provided by the new damage reporting mobile
                          application, which happened to be released shortly before St. Himark was
                          hit by the earthquake. This app allows citizens to provide information
                          about the damages occurred in relation to the earthquake, facilitating the
                          hard work of the emergency services.
                        </p>
                        <h6><strong>Answer 1</strong></h6>
                        <p class="text-justify">
                          The web app answers to the first question through the pie chart and the
                          stacked bar chart. Indeed, the pie chart represents the mean of the
                          damages plus the mean of the shake intensity, which is deemed to be
                          fundamental data that needs to be taken into consideration along with
                          the other information in order to find any discrepancy with the data
                          provided by the citizens reports. <br>
                          As it is possible to observe on the stacked bar chart the locations
                          that were hit the hardest from April 6th to April 11th (considering a
                          timeframe that goes from 12AM to 11PM), were: location 3 (which had
                          the highest average of damages and shake intensity), 11, 9, 8, 10, and 14.
                          Therefore it is essential to prioritize the neighborhoods that
                          result in having a high average of damages since not every neighborhood
                          that experienced a significant shake intensity reported a notable
                          amount of them.
                        </p>
                        <h6><strong>Answer 2</strong></h6>
                        <p class="text-justify">
                          The second question is answered by the boxplot, which shows the
                          reliability of the reports highlighting any possible outlier
                          outside the boxplot whiskers. The bigger is the box and the
                          highest is the number of outliers, the less reliable and consistent
                          are the reports provided from that neighborhood. <br> Therefore,
                          the neighborhoods that are stating the most reliable reports are
                          those that represent the variables (medical, power, buildings and
                          etc) in a reduced size of the box and a low to none
                          number of outliers. <br>
                          However, it is also crucial to take into account the number of reports
                          submitted, indeed a considerable size of the box with an high number
                          of reports could indicate low accuracy and reliability in the declared
                          values. <br>
                          Considering the previous timeframe and period, the neighborhoods that
                          presented the most credible and thus reliable reports are 13 and 16.
                        </p>
                        <h6><strong>Answer 3</strong></h6>
                        <p class="text-justify">
                          The last question concerns the condition and uncertainty changes of
                          the data. In order to examine the variations, the timeline and the
                          boxplot were the most efficient tool to use.
                          The results gathered show that there was a progressive increase in
                          the amount of outliers as well as in the number of reports, especially
                          from April 8th to April 9th, while the box size seems to remain modest.
                          However, the last two days (April 10-11th) the reports show signs of a
                          steady decrease. <br>
                          Indeed the amount of outliers diminishes too, meanwhile there's an
                          increase in the size of the box, indicating a significant scattering
                          of the values.
                        </p>
                      </div>
                    </b-sidebar>
                  </div>
                 <b-col>
                <b-collapse block variant="secondary" id="collapse-2" class="mb-3">
                    <b-card border-variant="secondary">
                      <h5>Map of St. Himark neighbourhoods</h5>
                      <b-col>
                        <b-card-body>
                            <img alt="Location map image" src="./assets/locationmap.png">
                        </b-card-body>
                      </b-col>
                      <b-tabs card>
                        <b-tab title="Pre-quake map" active>
                          <h6>The pre-quake map is from April 6th.</h6>
                          <img alt="prequake map image" src="./assets/mc1-prequake-shakemap.png">
                        </b-tab>
                        <b-tab title="Major quake map">
                          <h6>The major quake map is from April 8th.</h6>
                          <img alt="major quake map image"
                               src="./assets/mc1-majorquake-shakemap.png">
                        </b-tab>
                      </b-tabs>
                    </b-card>
                </b-collapse>
                  </b-col>
                </b-row>
              </div>
        </b-col>
      </b-row>
      <b-row>
        <b-col>
          <b-card
            border-variant="secondary"
            header="Mean of Damages"
            header-border-variant="secondary"
            class="mb-3">
            <b-card-body align="left">
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
            <b-card-footer align="left">
              <h6>Number of reports: {{ nReports }} </h6>
            </b-card-footer>
          </b-card>
        </b-col>
      </b-row>
      <b-card border-variant="secondary"
              header="Stacked Bar Chart"
              header-border-variant="secondary"
      class="mb-3">
      <Stackedbar :bindingStacked="dataStacked"></Stackedbar>
      </b-card>
      <b-card border-variant="secondary"
              header="Boxplot"
              header-border-variant="secondary"
              class="mb-3">
      <Boxplot :bindingBox="dataBox"></Boxplot>
      </b-card>
      <b-card border-variant="secondary"
              header="Timeline Slider"
              header-border-variant="secondary"
              class="mb-3">
      <b-row>
        <div>
          <b-button :pressed.sync="myButtonTimeline" variant="dark" class="ml-3 mb-4"
                    @click="disable = !disable">
            {{ disable ? 'Disable' : 'Enable' }} timeline
          </b-button>
<!--          <p> <strong>{{ myButtonTimeline }}</strong></p>-->
        </div>
      </b-row>
      <b-row>
        <b-col>
          <div>
<!--            <label class="mb-4">Timeline</label>-->
            <b-form-input id="range-1" v-model="slider.value" type="range" min="0"
                          :max="finalstate"
                          step="5"></b-form-input>
            <div class="mt-2">hours: {{ slider.time }}</div>
          </div>
        </b-col>
        </b-row>
      </b-card>
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
