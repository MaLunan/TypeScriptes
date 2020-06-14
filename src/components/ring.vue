<template>
  <div id="ring" ref="ring" style="width: 100%;height:100%;"></div>
</template>

<script lang="ts">
import { Vue, Component, Prop, Emit, Watch } from 'vue-property-decorator';
import Echarts from 'echarts';
import data from '../views/data';
@Component({
  components: {},
})
export default class Filtercondition extends Vue {
  private dom: any;
  private myChart: any;
  // 原有的日期值
  private parData: any;
  // 展示的 带 “”的日期值
  private enData: any;
  // name名
  private parId: any;
  private showDatas: any;
  private getData() {
    setTimeout((item: any) => {
      // 处理数据
      this.dataSet(data.ringData);
      this.creatEcharts();
    }, 1000);
  }
  private dataSet(ringData: []) {
    const ber: any = [];
    this.parData = ringData.forEach((res: any) => {
      if (ber.indexOf(res.groupBy) === -1) {
        ber.push(res.groupBy);
      }
    });
    this.parData = JSON.parse(JSON.stringify(ber));
    const berId: any = [];
    ringData.forEach((res: any) => {
      if (berId.indexOf(res.eventName) === -1) {
        berId.push(res.eventName);
      }
    });
    this.parId = berId;
    this.enData = ber.map((val: any) => {
      console.log(val);
      return (val.slice(0, 4) + '-' + val.slice(4).slice(0, 7)).slice(0, 7)
        + '-' + (val.slice(0, 4) + '-' + val.slice(4).slice(0, 7)).slice(7);
    });
    if (this.enData.length < 7) {
      this.enData.length = 7;
      for (let i: any = 0;
        i < this.enData.length;
        i++) {
        if (!this.enData[i]) {
          this.enData[i] = ' ';
        }
      }
    }
    const showData: any = {};
    console.log(this.parData);
    // 循环tabel分组的值
    this.parId.forEach((id: any) => {
      // 循环总数据
      ringData.forEach((coo: any, index: number) => {
        // 循环日期的值
        this.parData.map((clo: any) => {
          if (coo.eventName === id && coo.groupBy === clo) {
            if (showData[id]) {
              showData[id].push(coo.attributeRatio);
            } else {
              showData[id] = [];
              showData[id].push(coo.attributeRatio);
            }
          }
        });
      });
    });
    // 循环三个分组
    this.showDatas = this.parId.map((idd: any) => {
      return {
        type: 'bar',
        data: showData[idd],
        coordinateSystem: 'polar',
        name: idd,
        stack: 'a'      };
    });
    console.log(this.showDatas);
  }
  private creatEcharts() {
    const el: any = this.$refs.ring;
    this.myChart = Echarts.init(el);
    // 绘制图表
    this.myChart.setOption({
      angleAxis: {
        type: 'category',
        data: this.enData,
      },
      radiusAxis: {
      },
      polar: {
      },
      series: this.showDatas,
      legend: {
        show: true,
        data: this.parId,
      },
    });
  }
}
</script>
<style lang="scss" scoped>
</style>
