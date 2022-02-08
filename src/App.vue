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
    <!-- The API is returning incomplete data for recovered -->
    <!-- <div class="row mt-5" v-if="arrRecovered.length > 0">
      <div class="col">
        <h2>Recovered</h2>
        <line-chart :chartData="arrRecovered" :options="chartOptions" label="Recovered" :chartColors="recoveredColors"> </line-chart>
      </div>
    </div> -->
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
        pointBackgroundColor: "rgba(195, 37, 48, 1)",
        backgroundColor: "rgba(195, 37, 48, 0.1)",
      },
      arrHospitalized: [],
      hospitalizedColors: {
        borderColor: "#442B48",
        pointBorderColor: "#0E1428",
        pointBackgroundColor: "rgba(241, 232, 184, 1)",
        backgroundColor: "rgba(241, 232, 184, 0.1)",
      },
      arrInIcu: [],
      inIcuColors: {
        borderColor: "#423E28",
        pointBorderColor: "#423E28",
        pointBackgroundColor: "rgba(99, 185, 149, 1)",
        backgroundColor: "rgba(99, 185, 149, 0.1)",
      },
      arrOnVentilators: [],
      onVentilatorsColors: {
        borderColor: "#3d5a80",
        pointBorderColor: "#3d5a80",
        pointBackgroundColor: "rgba(152, 193, 217, 1)",
        backgroundColor: "rgba(152, 193, 217, 0.1)",
      },
      arrRecovered: [],
      recoveredColors: {
        borderColor: "#0081a7",
        pointBorderColor: "#0081a7",
        pointBackgroundColor: "rgba(0, 175, 185, 1)",
        backgroundColor: "rgba(0, 175, 185, 0.1)",
      },
      arrDeaths: [],
      deathColors: {
        borderColor: "#E06D06",
        pointBorderColor: "#E06D06",
        pointBackgroundColor: "rgba(64, 42, 44, 1)",
        backgroundColor: "rgba(64, 42, 44, 0.2)",
      },
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
      }
    };
  },
  async created() {
    const { data } = await axios.get("https://api.covidtracking.com/v1/us/daily.json"); // temporal for:"https://covidtracking.com/api/us/daily"

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
      // console.log(this.arrRecovered);
  },
};
</script>

