<template>
  <div>
    <div v-if="loder">
      <div class="text-center">
      <v-icon
      large
      color="green darken-2"
    >
      mdi-currency-eur 
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
      <Tabla :datos="euros" />
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Tabla from '@/components/Tabla'

export default {
  name: 'Euro',
  components: {
    Tabla
  },

  data: () => ({
    loder: true,
    euros: [],
  }),

  created() {
    this.euroData();
  },

  mounted() {
    this.grafico()
  },

  methods: {

    async euroData() {
      try {
        const {data: datos} = await axios.get('https://mindicador.cl/api/euro')
        this.euros = await datos.serie
        // console.log('datos euro.serie: ', this.euros)
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
            text: 'Euro'
          },
          data: [
            {
              type: 'spline',
              dataPoints: dataPoints
            }
          ]
        })
        for (var i = 0; i < this.euros.length; i++) {
          dataPoints.push({
            x: new Date(this.euros[i].fecha),
            y: this.euros[i].valor
          })
        }
        this.loder = false

        chart.render()
      }, 1000)
    }
  }
}
</script>
