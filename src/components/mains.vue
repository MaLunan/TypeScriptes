<template>
  <div id="mains" ref="mains" style="width: 100%;height:900px;"></div>
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
  private parData: any;
  // 展示的 带 “”的日期值
  private enData: any;
  // name名
  private parId: any;
  private showDatas: any;
  //   @Prop()
  //   private filterData!: any; // 高级设置且或按钮事件
  //   changebtn (info: number) {
  //     this.andbtn = info;
  //   };
  //   @Emit('filterCancel')
  //   sendCancel(treedata: [], ar: number) {
  //     const msg = {
  //       treedata,
  //       ar,
  //     };
  //     return msg;
  //   }
  //   filterFalse() {
  //     this.sendCancel(this.treedatas, this.andbtns);
  //   } // 保存按钮
  //   private mounted() {

  //   }
  private getData() {
    setTimeout((item: any) => {
      // 处理数据
      this.dataSet(data.mainsData);
      this.creatEcharts();
    }, 1000);
  }
  private dataSet(ringData: any) {
    const ber: any = [];
    for (const i in ringData) {
      if (ringData.hasOwnProperty(i)) {
        ringData[i].forEach((item: any) => {
          if (ber.indexOf(item.groupBy) === -1) {
            ber.push(item.groupBy);
          }
        });
      }
    }
    this.parData = JSON.parse(JSON.stringify(ber));
    const berId: any = [];
    for (const i in ringData) {
      if (ringData.hasOwnProperty(i)) {
        ringData[i].forEach((item: any) => {
          if (berId.indexOf(item.eventName) === -1) {
            berId.push(item.eventName);
          }
        });
      }
    }
    this.parId = berId;
    // this.enData = ber.map((val: any) => {
    //   console.log(val);
    //   return (val.slice(0, 4) + '-' + val.slice(4).slice(0, 7)).slice(0, 7)
    //     + '-' + (val.slice(0, 4) + '-' + val.slice(4).slice(0, 7)).slice(7);
    // });
    // if (this.parData.length >= 5) {
    //   this.parData.splice(4, this.parData.length - 5);
    // }
    const showData: any = {};
    console.log(this.parId);
    // 循环日期的值
    this.parData.map((clo: any) => {
      // 循环tabel分组的值
      //   this.parId.forEach((i: any) => {
      // 循环总数据

      for (const i in ringData) {
        if (ringData.hasOwnProperty(i)) {
          let bj: any = true;
          ringData[i].forEach((coo: any, index: number) => {
            if (coo.eventName === i && coo.groupBy === clo) {
              if (showData[clo]) {
                showData[clo].push(coo.attributeRatio);
                bj = false;
              } else {
                showData[clo] = [];
                showData[clo].push(coo.attributeRatio);
                bj = false;
              }
            } else if (ringData[i].length - 1 === index) {
              if (showData[clo]) {
                if (bj) {
                  showData[clo].push('none');
                }
                return;
              } else {
                showData[clo] = [];
                showData[clo].push('none');
              }
            }
          });
        }
      }
      //   });
    });
    console.log(showData, 'showData');
    // 循环三个分组
    this.showDatas = this.parData.map((idd: any, index: any) => {
      return {
        name: this.parData[index],
        type: 'bar',
        stack: '%',
        label: {
          show: true,
          position: 'insideRight',
        },
        data: showData[idd],
      };
    });
    console.log(this.showDatas);
  }
  private creatEcharts() {
    const el: any = this.$refs.mains;
    this.myChart = Echarts.init(el);
    // 绘制图表
    this.myChart.setOption({
      tooltip: {
        trigger: 'axis',
        axisPointer: {            // 坐标轴指示器，坐标轴触发有效
          type: 'shadow',        // 默认为直线，可选为：'line' | 'shadow'
        },
        formatter: (val: any) => {
          const cos: any = [];
          val.forEach((ver: any) => {
            if (ver.value === 'none') {
              return;
            }
            cos.push(ver.marker + ver.seriesName + ': ' + (ver.value * 100).toFixed(2) + '%');
          });
          return val[0].name + '<br/>' + cos.join('<br/>');
        },
      },
      legend: {
        data: this.parData,
      },
      grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true,
      },
      xAxis: {
        type: 'value',
      },
      yAxis: {
        type: 'category',
        data: this.parId,
      },
      series: this.showDatas    });
  }
}
</script>
<style lang="scss" scoped>
</style>
