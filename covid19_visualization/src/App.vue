<template>
  <div id="app" class="container">
    <div class="row mt-5" v-if="arrPositive.length > 0">
      <div class="col">
        <h2>Positive Increase per Day</h2>
        <line-chart :chartData="arrPositiveInc" :options="chartOptions" label="Positive Increase"></line-chart>
        
        <h2>Positive Total</h2>
        <line-chart :chartData="arrPositive" :options="chartOptions" label="Positive"></line-chart>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'
import LineChart from './components/LineChart'

export default {
  name: 'App',
  components:{
    LineChart
  },
  data() {
    return {
      arrPositive: [],
      arrHospitalized: [],
      arrInICU: [],
      arrOnVentilator: [],
      arrRecovered: [],
      arrDeath: [],

      arrDeathInc: [],
      arrPositiveInc: [],

      chartOptions: {
          responsive: true,
          maintainAspectRatio: false,
      }
    }
  }, 
  async created() {
    const { data } = await axios.get("https://covidtracking.com/api/us/daily")
    console.log(data)
    data.forEach(d => {
      const date = moment(d.date, "YYYYMMDD").format("MM/DD")

      const {
        positive,
        hospitalizedCurrently,
        inIcuCurrently,
        onVentilatorCurrently,
        recovered,
        death,
        positiveIncrease,
        deathIncrease,
      } = d

      this.arrPositive.push({date, total: positive})
      this.arrHospitalized.push({date, total: hospitalizedCurrently})
      this.arrInICU.push({date, total: inIcuCurrently})
      this.arrOnVentilator.push({date, total: onVentilatorCurrently})
      this.arrRecovered.push({date, total: recovered})
      this.arrDeath.push({date, total: death})
      this.arrPositiveInc.push({date, total: positiveIncrease})
      this.arrDeathInc.push({date, total: deathIncrease})
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.col {
  height: 100vh;
}
</style>
