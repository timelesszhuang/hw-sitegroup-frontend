<template>
  <div class="echarts">
    <IEcharts :option="bar" :loading="loading" @ready="onReady" @click="onClick"></IEcharts>
  </div>
</template>
<script type="text/babel">
  import IEcharts from 'vue-echarts-v3/src/full.vue';
  import http from '../../../assets/js/http.js';
  export default {
    name: 'view',
    components: {
      IEcharts,
    },
    props: {},
    data: () => ({
      loading: false,
      bar: {
        color: ["#BBFFFF"],
        tooltip: {
          trigger: 'axis',
          formatter: '{b} <br/>{a}:{c}篇',
          axisPointer: {            // 坐标轴指示器，坐标轴触发有效
            type: 'shadow'        // 默认为直线，可选为：'line' | 'shadow'
          }
        },
        grid: {
          left: '20%',
          right: '25%',
          containLabel: true

        },
        title: {
          text: '问答统计',
          left: 'center',
          top: 10
        },
        xAxis: [
          {
            type: 'category',
            data: [],
            axisLabel: {
              interval: 0,
              rotate: -20,


            }
          }
        ],
        yAxis: [
          {
            type: 'value'
          }
        ],
        series: [
          {
            name: '共',
            type: 'bar',
            barWidth: '17%',
            data: [],
          }
        ]
      },

    }),
    methods: {
      init() {
        this.doRandom();
      },
      queryData() {
        this.doRandom();
      },
      doRandom() {
        const that = this;
        this.apiGet('questiontype/QuestionCount').then((data) => {
          this.handelResponse(data, (data, msg) => {
            that.bar.series[0].data = data.count;
            that.bar.xAxis[0].data = data.name;
          }, (data, msg) => {
            this.$Message.error(msg);
          })
        },)

//        that.loading = !that.loading;
      },
      onReady(instance) {
      },
      onClick(event, instance, echarts) {
      }
    },
    mixins: [http]

  };
</script>

<style scoped>
  .echarts {
    width: 98%;

    height: 500px;
    float: left;
    padding-bottom: 50px;
  }
</style>
