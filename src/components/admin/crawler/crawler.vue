<template>
  <div class="echarts">
    <div style="width: 700px;height: 20px;margin: 0 auto">
      <Row>
        <Col span="9">
        <Date-picker type="daterange" v-model="time" placement="bottom-end" placeholder="选择日期查询"></Date-picker>
        </Col>
        <Select v-model="site_id" style="width:310px;" label-in-value filterable clearable>
          <Option v-for="item in site" :value="item.id" :label="item.text" :key="item">
            {{ item.text }}
          </Option>
        </Select>
        &nbsp;<Button type="primary" @click="queryData">查询</Button>
      </Row>
    </div>
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
      site: [],
      time:[],
      site_id: "",
      loading: false,
      data: [],
      bar: {
        color: ["#20a0ff", "#13CE66", "#F7BA2A", "#FF4949", "#61a0a8", "#BBFFFF", "#FF1493", "#FF34B3", "#FF00FF", '#FFDAB9'],
        title: {
          text: '爬虫统计',
          left: 'center',
          top: 10
        },
        tooltip: {
          trigger: 'axis'
        },
        legend: {
          bottom: 'bottom',
          data: []
        },
        grid: {
          left: '5%',
          right: '25%',
          bottom: '20%',
          containLabel: true

        },

        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: [],
          axisLabel: {
            interval: 0,
            rotate: -25,
          },
          axisLine: {show: false},
          splitNumber: 100,
          splitLine: {
            show: true,
          },
        },
        yAxis: {
          type: 'value'
        },
        series: []
      },

    }),
    methods: {
      init() {
        this.doRandom();
        this.getSite((data) => {
          this.site = data
        });
      },
      getSite(){
        this.apiGet('Site/getSites').then((res) => {
          this.handelResponse(res, (data, msg) => {
            this.site = data
//            console.log(this.site)
          }, (data, msg) => {
            this.$Message.error(msg);
          })
        }, (res) => {
          //处理错误信息
          this.$Message.error('网络异常，请稍后重试。');
        })
      },
      queryData() {
        this.doRandom();
        this.getSite((data) => {
          this.site = data
        });
      },
      doRandom() {
        const that = this;
        let data = {
          params: {
            time: this.time,
            site_id: this.site_id,
          }
        }
        this.apiGet('count/enginecount', data).then((data) => {
          this.handelResponse(data, (data, msg) => {
            that.bar.legend.data = data.type;
            that.bar.xAxis.data = data.time;
            that.bar.series = data.type;
            that.bar.series[0].areaStyle = {normal: {}};
            that.bar.series[1].areaStyle = {normal: {}};
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
    width: 800px;
    height: 600px;
    margin: 0 auto;
    padding-bottom: 50px;
  }
</style>
