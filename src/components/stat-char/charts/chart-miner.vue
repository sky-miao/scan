// template define
<template>
    <div class='grid-content bg-purple'>
      <div id='statChartsMiner' class="statChartsClass" :style="{ 'min-width': '300px', width: '835px', height: '500px'}"></div>
    </div>
</template>

// script define
<script>
import { mapActions } from 'vuex'
import _ from 'lodash'
import { formatNumber } from '../../../untils/format'
export default {
  mounted () {
    this.getMinerChart()
    this.drawStatChartsMiner()
  },
  computed: {
    statData: {
      get () {
        return this.$store.state.chart.minerChart
      }
    }
  },
  watch: {
    statData: {
      handler: function (val, oldval) {
        this.drawStatChartsMiner()
      }
    }
  },
  filters: {
    numberFilter (value) {
      return formatNumber(value)
    }
  },
  methods: {
    ...mapActions(['getMinerChart']),
    drawStatChartsMiner () {
      // charts locate
      var statChartsElement = document.getElementById('statChartsMiner')
      // charts instantiatte
      var statCharts = this.$echarts.init(statChartsElement)
      if (_.isUndefined(this.statData.seriesData) || this.statData.seriesData.length === 0) {
        statCharts.showLoading({
          text: this.$t('statcharts.common.loading')
        })
      } else {
        statCharts.hideLoading()
      }
      // method define:
      //    getting data
      //    tip display
      // define  charts option
      var option = {
        toolbox: {
          show: true,
          feature: {
            mark: {show: true},
            dataView: {show: true, readOnly: false},
            magicType: {
              show: true,
              type: ['pie', 'funnel']
            },
            restore: {show: true},
            saveAsImage: {show: true}
          }
        },
        tooltip: {
          trigger: 'item',
          formatter: '{b} ({d}%) <br/> ' + this.$t('statcharts.miner.minerByBlockTipname') + ': {c} '
        },
        legend: {
          type: 'scroll',
          orient: 'vertical',
          x: 'left',
          right: 10,
          top: 20,
          bottom: 20,
          data: this.statData.legendData
        },
        series: [
          {
            name: this.$t('statcharts.miner.minerByBlockName'),
            type: 'pie',
            smooth: true,
            symbol: 'circle',
            symbolSize: 5,
            sampling: 'average',
            radius: '40%',
            center: ['50%', '50%'],
            itemStyle: {
              emphasis: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            },
            data: this.statData.seriesData
          }
        ]
      }
      // setting charts option
      statCharts.setOption(option)
    }
  }
}
</script>
<style>
  @import "../../../assets/css/chars.less";
</style>
