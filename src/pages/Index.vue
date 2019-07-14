<template>
  <q-page class="flex">
    <div class="tempo" v-if="tempo.result">
      <h6 class="data-hora">{{tempo.result.dthr_log | date}}</h6>
      <div class="tempo-agora row">
        <div class="col temperatura"><h3>{{tempo.result.temperatura}}</h3> °c</div>
        <div class="col chovendo">
          <span v-if="tempo.result.rain_fall > 0">Choveu {{parseFloat(tempo.result.rain_fall).toFixed(2)}} mm<q-icon name="fas fa-cloud-sun-rain"></q-icon></span>
          <span v-if="tempo.result.rain_fall <= 0">Não choveu <q-icon name="fas fa-cloud-sun"></q-icon></span>
        </div>
      </div>
      <div class="previsao" v-if="previsao.result">
        <h4>Previsão</h4>
        <div class="row">
          <div class="col">
            <span>Hoje </span>
          </div>
          <div class="col">
            <span v-if="previsao.result.previsao_dia.chuva">Chuva {{parseFloat(previsao.result.previsao_dia.precipitacao).toFixed(2)}} mm</span>
            <span v-if="!previsao.result.previsao_dia.chuva">Sem chuva</span>
          </div>
        </div>
        <div class="row">
          <div class="col">
            <span>{{previsao.result.dt_log_turno}} hrs </span>
          </div>
          <div class="col">
            <span v-if="previsao.result.previsao_turno.chuva">Chuva {{parseFloat(previsao.result.previsao_turno.precipitacao).toFixed(2)}} mm</span>
            <span v-if="!previsao.result.previsao_turno.chuva">Sem chuva</span>
          </div>
        </div>
        <div class="row">
          <div class="col">
            <span>{{previsao.result.dt_log_hora}}:00 as {{parseInt(previsao.result.dt_log_hora) + 1}}:00 hrs </span>
          </div>
          <div class="col">
            <span v-if="previsao.result.previsao_hora.chuva">Chuva {{parseFloat(previsao.result.previsao_hora.precipitacao).toFixed(2)}} mm</span>
            <span v-if="!previsao.result.previsao_hora.chuva">Sem chuva</span>
          </div>
        </div>
      </div>
    </div>
  </q-page>
</template>

<style lang="stylus">
  .q-header
    background #00aaaa
  .q-page
    background #00aaaa
    color white
  .tempo
    padding 5%
    max-width 800px
    width 100%
    margin auto
    margin-top 10px
  .temperatura
    h3
      display inline-block
  .chovendo
    margin-left 50px
    margin-top 27px
  .previsao
    margin-top 50px
  .chovendo .q-icon
    font-size: 60px;
    display: block;
</style>

<script>
import { date } from 'quasar'

export default {
  name: 'PageIndex',
  data () {
    return {
      tempo: {},
      previsao: {}
    }
  },
  filters: {
    date (value) {
      return date.formatDate(value, 'DD MMMM HH:mm')
    }
  },
  created () {
    let self = this
    this.$axios.get('https://comercial.eleventickets.com/Weather/WnPrevisaoTempoClass/getLog/056db757bccca12bb306b67538a58a07')
      .then((response) => {
        self.$data.tempo = response.data
      })
      .catch(() => {
        this.$q.notify({
          color: 'negative',
          position: 'top',
          message: 'Loading failed',
          icon: 'report_problem'
        })
      })

    this.$axios.get('https://comercial.eleventickets.com/Weather/WnPrevisaoTempoClass/getPrevisao/056db757bccca12bb306b67538a58a07')
      .then((response) => {
        self.$data.previsao = response.data
        console.log(response.data)
      })
      .catch(() => {
        this.$q.notify({
          color: 'negative',
          position: 'top',
          message: 'Loading failed',
          icon: 'report_problem'
        })
      })
  }
}
</script>
