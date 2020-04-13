<template>
  <div>
    <div id="highchart" ></div>
    
    <select v-model="country" @change = 'countrySelector()'>
      <option value="russia">россия</option>
      <option value="italy">италия</option>
      <option value="uganda">Уганда</option>
      <option value="ukraine">украина</option>
      <option value="india">индия</option>
      <option value="moldova">молдова</option>
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
<script src="/js/themes/dark-blue.js"></script>
<script>
  import axios from 'axios'
  import moment from 'moment'
  
  import Highcharts from 'highcharts'
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
      async getData(country, status, cases) {
       return await axios
          .get(`https://api.covid19api.com/country/${country}/status/${status}`) 
          .then(response => {
            this.data = response.data
            this.arrLength = response.data.length-14
            response.data.slice(this.arrLength).forEach(e => cases.push(e.Cases))
            //console.table(response.data)


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
        
              Highcharts.chart('highchart', {
    chart: {
        type: 'areaspline',
        backgroundColor: {
            linearGradient: [0, 0, 0, 500],
            stops: [
                [0, 'rgb(65, 66, 68)'],
                [1, 'rgb(170, 170, 170)'],

            ]
        },
    },
    colors: ['#4f50fd', '#28cc2a', '#cc2a2a'],
    title: {
        text: ''
    },
    legend: {
        layout: 'vertical',
        align: 'left',
        verticalAlign: 'top',
        x: 150,
        y: 100,
        floating: true,
        borderWidth: 1,
        backgroundColor:
            Highcharts.defaultOptions.legend.backgroundColor || 'rgb(170, 170, 170)'
    },
    xAxis: {
        
        categories: this.date,
        
        
 
        
    },
    yAxis: {
        title: {
            text: ''
        }
    },
    tooltip: {
        shared: true,
        valueSuffix: ' человек'
    },
    credits: {
        enabled: true
    },
    plotOptions: {
        areaspline: {
            fillOpacity: 0.5
        }
    },
    series: [{
        name: 'зараженные',
        data: this.confirmed
    }, {
        name: 'вылечено',
        data: this.recovered
    },{
        name: 'смерти',
        data: this.deaths
    }]
});
  

      }
    },
  }
</script>
