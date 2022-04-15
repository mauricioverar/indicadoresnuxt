<template>
  <div>
    <div v-if="loder">
      <div class="text-center">
        <v-icon large color="green darken-2"> mdi-currency-btc </v-icon>
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
      <Tabla :datos="bitcoin" />
    </div>
  </div>
</template>

<script>
import Tabla from "@/components/Tabla";
import axios from "axios";

export default {
  name: "Bitcoin",
  components: { Tabla },

  data: () => ({
    loder: true,
    bitcoin: [],
  }),

  created() {
    this.bitcoinData();
  },

  mounted() {
    this.grafico();
  },

  methods: {
    async bitcoinData() {
      // console.log('datos bitcoin ')

      try {
        const { data: datos } = await axios.get(
          "https://mindicador.cl/api/bitcoin"
        );
        this.bitcoin = await datos.serie;
        // console.log("datos bitcoin.serie: ", this.bitcoin);
      } catch (error) {
        // console.log(error)
      }
    },

    grafico() {
      let loder = true;
      setTimeout(() => {
        var dataPoints = [];
        let chart = new CanvasJS.Chart("chartContainer", {
          theme: "light2",
          animationEnabled: false,
          title: {
            text: "Bitcoin",
          },
          data: [
            {
              type: "spline",
              dataPoints: dataPoints,
            },
          ],
        });
        for (var i = 0; i < this.bitcoin.length; i++) {
          dataPoints.push({
            x: new Date(this.bitcoin[i].fecha),
            y: this.bitcoin[i].valor,
          });
        }
        this.loder = false;

        chart.render();
      }, 1000);
    },
  },
};
</script>
