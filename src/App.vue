<template>
  <div id="app" class="container">
    <div class="row mt-5">
      <div class="col text-center">
        <h1>COVID-19 Data Visualization</h1>
      </div>
    </div>
    <div class="row mt-5" v-if="arrPositiv.length > 0">
      <div class="col">
        <h2>Positive</h2>
        <line-chart :chartData="arrPositiv" :options="chartOptions" label="Positive" :chartColors="positiveColors"> </line-chart>
      </div>
    </div>
    <div class="row mt-5" v-if="arrHospitalized.length > 0">
      <div class="col">
        <h2>Hospitalized</h2>
        <line-chart :chartData="arrHospitalized" :options="chartOptions" label="Hospitalized" :chartColors="hospitalizedColors"> </line-chart>
      </div>
    </div>
    <div class="row mt-5" v-if="arrInIcu.length > 0">
      <div class="col">
        <h2>In ICU</h2>
        <line-chart :chartData="arrInIcu" :options="chartOptions" label="In ICE" :chartColors="inIcuColors"> </line-chart>
      </div>
    </div>
    <div class="row mt-5" v-if="arrOnVentilators.length > 0">
      <div class="col">
        <h2>On Ventilators</h2>
        <line-chart :chartData="arrOnVentilators" :options="chartOptions" label="On Ventilatior" :chartColors="onVentilatorsColors"> </line-chart>
      </div>
    </div>
    <div class="row mt-5" v-if="arrRecovered.length > 0">
      <div class="col">
        <h2>Recovered</h2>
        <line-chart :chartData="arrRecovered" :options="chartOptions" label="Recovered" :chartColors="recoveredColors"> </line-chart>
      </div>
    </div>
    <div class="row mt-5" v-if="arrDeaths.length > 0">
      <div class="col">
        <h2>Deaths</h2>
        <line-chart :chartData="arrDeaths" :options="chartOptions" label="Deaths" :chartColors="deathColors"> </line-chart>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from 'moment';
import LineChart from './components/LineChart';

export default {
  name: "App",
  components: {
    LineChart
  },
  data() {
    return {
      arrPositiv: [],
      positiveColors: {
        borderColor: "#353652",
        pointBorderColor: "#353652",
        pointBackgroundColor: "#c32530",
        backgroundColor: "#c32530",
      },
      arrHospitalized: [],
      hospitalizedColors: {
        borderColor: "#442B48",
        pointBorderColor: "#0E1428",
        pointBackgroundColor: "#F1E8B8",
        backgroundColor: "#B1AE91",
      },
      arrInIcu: [],
      inIcuColors: {
        borderColor: "#161a1d",
        pointBorderColor: "#161a1d",
        pointBackgroundColor: "#a4161a",
        backgroundColor: "#a4161a",
      },
      arrOnVentilators: [],
      onVentilatorsColors: {
        borderColor: "#3d5a80",
        pointBorderColor: "#3d5a80",
        pointBackgroundColor: "#98c1d9",
        backgroundColor: "#98c1d9",
      },
      arrRecovered: [],
      recoveredColors: {
        borderColor: "#0081a7",
        pointBorderColor: "#0081a7",
        pointBackgroundColor: "#00afb9",
        backgroundColor: "#00afb9",
      },
      arrDeaths: [],
      deathColors: {
        borderColor: "#E06D06",
        pointBorderColor: "#E06D06",
        pointBackgroundColor: "#402A2C",
        backgroundColor: "#402A2C",
      },
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
      }
    };
  },
  async created() {
    const { data } = await axios.get("http://covidtracking.com/api/us/daily");
    // console.log(data.slice(0, 45));

    data.forEach( d => {
      const date = moment(d.date, "YYYYMMDD").format("MM/DD");

      const {
        positive,
        hospitalizedCurrently,
        inIcuCurrently,
        onVentilatorCurrently,
        recovered,
        death,
      } = d;

      this.arrPositiv.push({date, total: positive});
      this.arrHospitalized.push({date, total: hospitalizedCurrently });
      this.arrInIcu.push({ date, total: inIcuCurrently });
      this.arrOnVentilators.push({ date, total: onVentilatorCurrently });
      this.arrRecovered.push({ date, total: recovered });
      this.arrDeaths.push({ date, total: death });

    });
      console.log(this.arrRecovered);
  },
};
</script>

<style>
/* #app {

} */
</style>

