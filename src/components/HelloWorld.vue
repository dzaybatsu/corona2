<template>
  <div>
    <div style="width: 50%; height: 50%"><canvas id="myChart"></canvas></div>
    <select v-model="country" @change = 'countrySelector()'>
      <option value="russia">Рашка</option>
      <option value="italy">Ухан</option>
      <option value="uganda">Уганда</option>
      <option value="ukraine">Европа</option>
      <option value="india">Еще одна</option>
      <option value="moldova">Третья молдова</option>
    </select>
    <table>

    </table>
  
    <section v-if="errored">
      <p>ты все сломал</p>
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
      deaths:[],
      recovered:[],
      country: 'russia',
      arrLength:'',
      info: null,
      loading: true,
      errored: false
    }),

    mounted() {
      
      this.getDate()
      this.getData(this.country, 'confirmed', this.confirmed)
      this.getData(this.country, 'recovered', this.recovered)
      this.getData(this.country, 'deaths', this.deaths)
     

    },

    methods: {
      getData(country, status, cases) {
        axios
          .get(`https://api.covid19api.com/country/${country}/status/${status}`) 
          .then(response => {
            this.data = response.data
            this.arrLength = response.data.length-14
            response.data.slice(this.arrLength).forEach(e => cases.push(e.Cases))
            console.table(response.data)
          })
      
        
        .catch(error => {
            this.errored = true
          })
          .finally(() => {
            this.loading = false
            this.renderChart()
           
            
            //console.table(this.data)
          })
      },
      getDate(){
        for (let i = 0; i < 14; i++ ) { 
            this.date.push(moment().subtract(i, 'days').format('DD-MM-YY'))
            }
            this.date.reverse()
            

      },

      countrySelector(){
        this.confirmed = []
        this.recovered = []
        this.deaths = []
        this.date = []
        this.getDate()
        this.getData(this.country, 'confirmed', this.confirmed)
        this.getData(this.country, 'recovered', this.recovered)
        this.getData(this.country, 'deaths', this.deaths)
      },

      

      renderChart() {
        
        let element = document.getElementById('myChart')//.getContext('2d')

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
               data: this.deaths,
            }
            ]
            
          }
        })
      }
    },
  }
</script>
