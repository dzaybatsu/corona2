<template>
  <div>
    <canvas id="myChart"></canvas>
    <!-- <button @click="dataDeaths()"></button> -->
    <table>
      <!-- <tr>
        <td>страна:  </td>
        <td>{{country}}</td>
      </tr>
      <tr>
        <td>подтверженно:  </td>
        <td>{{ confirmed }}</td>
      </tr>
      
      <tr>
        <td>смерти:  </td>
        <td>{{deaths}}</td>
      </tr>
      <tr>
        <td>вылечено:  </td>
        <td>{{date}}</td>
      </tr> -->
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
      data:'',
      
      curDate: '',
      date: [],
      
      status: 'confirmed',
      confirmed:[],
      deaths:'',
      recovered:'',
      country: 'russia',
      arrLength:'',
      info: null,
      loading: true,
      errored: false
    }),

    mounted() {
      
      
      this.getData(this.country, 'confirmed')
      //this.getData(this.country,'recovered')
      //this.getData(this.country,'deaths')
      //this.date = moment().push('DD-MM')//.subtract(7, 'days')//.format('DD-MM-YY')//
      //this.date = moment([2010, 1, 14])//.subtract(7, 'days')//.format('DD-MM-YY')//
    },

    methods: {
      getData(country, status) {
        axios
          .get(`https://api.covid19api.com/country/${country}/status/${status}`) 
          .then(response => {
            this.data = response.data
            this.arrLength = response.data.length-14
            response.data.slice(this.arrLength).forEach(element => this.confirmed.push(element.Cases))
            for (let i = 0; i < 14; i++ ) { 
            this.date.push(moment().subtract(i, 'days').format('DD-MM-YY'))
            }
            this.date.reverse()
          })
      
        
        .catch(error => {
            this.errored = true
          })
          .finally(() => {
            this.loading = false
            
            this.renderChart()
          })
      },

      

      renderChart() {
        
        let element = document.getElementById('myChart').getContext('2d')

        let chart = new Chart(element, {
          type: 'line',
          data: {
             labels: this.date,
            datasets: [{
              label: ['зараженные'],
              backgroundColor: '',
              borderColor: 'rgb(0, 0, 230)',
               data: this.confirmed,
            },{
              label: ['вылечено'],
              backgroundColor: '',
              borderColor: 'rgb(0, 230, 0)',
               data: this.recovered,
            },{
              label: ['смерти'],
              backgroundColor: '',
              borderColor: 'rgb(230, 0, 0)',
               data: this.recovered,
            }
            ]
            
          }
        })
      }
    },
  }
</script>
