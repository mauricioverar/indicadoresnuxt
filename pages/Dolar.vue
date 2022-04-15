<template>
  <div>
    <div v-if="loder">
      <div class="text-center">
      <v-icon
      large
      color="green darken-2"
    >
      mdi-currency-usd 
    </v-icon>

        <v-progress-circular
          indeterminate
          color="primary"
        ></v-progress-circular>
      </div>
    </div>

    <div
      id="chartContainer"
      style="height: 370px; max-width: 920px; margin: 0px auto"
    ></div>
    <div>
      <Tabla :datos="dolars" />
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Tabla from '@/components/Tabla'

export default {
  name: 'Dolar',
  components: {
    Tabla
  },

  data: () => ({
    loder: true,
    dolars: [],
  }),

  created() {
    this.dolarData();
  },

  mounted() {
    this.grafico()
  },

  methods: {

    async dolarData() {
      try {
        const {data: datos} = await axios.get('https://mindicador.cl/api/dolar')
        this.dolars = await datos.serie
        // console.log('datos dolar.serie: ', this.dolars)
      } catch (error) {
        // console.log(error)
      }
    },

    grafico() {
      let loder = true
      setTimeout(() => {
        var dataPoints = []
        let chart = new CanvasJS.Chart('chartContainer', {
          theme: 'light2',
          animationEnabled: false,
          title: {
            text: 'Dolar'
          },
          data: [
            {
              type: 'spline',
              dataPoints: dataPoints
            }
          ]
        })
        for (var i = 0; i < this.dolars.length; i++) {
          dataPoints.push({
            x: new Date(this.dolars[i].fecha),
            y: this.dolars[i].valor
          })
        }
        this.loder = false

        chart.render()
      }, 1000)
    }
  }
}
</script>
