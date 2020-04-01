<template>
  <div>
    <canvas id="myChart"></canvas>
    <button @click="renderChart()"></button>
    <table>
      <tr>
        <td>подтверженно:  </td>
        <td>{{ confirmed }}</td>
      </tr>
      <tr>
        <td>смерти:  </td>
        <td></td>
      </tr>
      <tr>
        <td>вылечено:  </td>
        <td></td>
      </tr>
      <tr>
        <td>страна:  </td>
        <td></td>
      </tr>
    </table>
  
    <section v-if="errored">
      <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
    </section>
    <section v-else>
      <div v-if="loading">Loading...</div>
  
      <div
        v-else
        v-for="(currency, id) in info"
        :key="id"
        class="currency"
      >
        {{ confirmed }}:
        <span class="lighten">
          <span v-text="currency.symbol"></span>{{ confirmed }}
        </span>
      </div>
    </section>
  </div>
</template>
<script>
  import axios from 'axios'
  import moment from 'moment'
  import Chart from 'chart.js'
  moment.locale('ru')

  export default {
    filters: {
      currencydecimal(value) {
        return value.toFixed(2);
      }
    },

    data: () => ({
      response:'',
      curDate: '',
      date: '',
      curMonth: '',
      status: 'confirmed',
      confirmed:'',
      country: 'russia',
      arrLength:'',
      info: null,
      loading: true,
      errored: false
    }),

    mounted() {
      this.date = moment().utc().format('YYYY-MM-DD') // 2020-04-01
      //this.date = new Date()
      this.curMonth = moment().format('MMMM')
      this.getData(this.country, this.status)
      //this.renderChart()
    },

    methods: {
      getData(country, status) {
        axios
          .get(`https://api.covid19api.com/total/country/${country}/status/confirmed`) 
          .then(response => {
            
            this.response = response.data
            this.arrLength = response.data.length-1
            this.confirmed = response.data[this.arrLength].Cases
            this.date = response.data[this.arrLength].Date
            this.renderChart()
            //this.confirmed = response.data.find(e => e.Date.search(this.moment().utc().format('YYYY-MM-DD')) != -1)
            //console.log(this.response.data.find(e => e.Date.search(this.moment().utc().format('YYYY-MM-DD')) != -1))

            console.log(this.date)
            //console.log(response.data[])
            //console.table(response.data)
          })
          .catch(error => {
            this.errored = true
          })
          .finally(() => {
            this.loading = false
            //this.renderChart2()
          })
      },

      renderChart2() {
        let element = document.getElementById('myChart').getContext('2d')

        let chart = new Chart(element, {
          type: 'line',
          data: [
            {x: 1, y: 1},
            {x: 1, y: 1},
            {x: 1, y: 1},
            {x: 1, y: 1},
            {x: 1, y: 1},
            {x: 1, y: 1},
          ],
        })
      },

      renderChart() {
        let element = document.getElementById('myChart').getContext('2d')

        let chart = new Chart(element, {
          type: 'line',
          data: {
             labels: [this.response[this.arrLength-13].Date.slice(0, 10),this.response[this.arrLength-12].Date.slice(0, 10),this.response[this.arrLength-11].Date.slice(0, 10),this.response[this.arrLength-10].Date.slice(0, 10),this.response[this.arrLength-9].Date.slice(0, 10),this.response[this.arrLength-8].Date.slice(0, 10),this.response[this.arrLength-7].Date.slice(0, 10),this.response[this.arrLength-6].Date.slice(0, 10),this.response[this.arrLength-5].Date.slice(0, 10),this.response[this.arrLength-4].Date.slice(0, 10), this.response[this.arrLength-3].Date.slice(0, 10), this.response[this.arrLength-2].Date.slice(0, 10), this.response[this.arrLength-1].Date.slice(0, 10), this.response[this.arrLength].Date.slice(0, 10)],
            datasets: [{
              label: ['зараженные'],
              backgroundColor: '',
              borderColor: 'rgb(0, 0, 0)',
               data: [this.response[this.arrLength-13].Cases,this.response[this.arrLength-12].Cases,this.response[this.arrLength-11].Cases,this.response[this.arrLength-10].Cases,this.response[this.arrLength-9].Cases,this.response[this.arrLength-8].Cases,this.response[this.arrLength-7].Cases,this.response[this.arrLength-6].Cases,this.response[this.arrLength-5].Cases,this.response[this.arrLength-4].Cases,this.response[this.arrLength-3].Cases,this.response[this.arrLength-2].Cases,this.response[this.arrLength-1].Cases,this.response[this.arrLength].Cases],
            }]
          }
        })
      }
    },
  }
</script>
