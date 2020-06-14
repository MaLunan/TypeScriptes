<template>
  <div id="main" ref="main" style="width: 100%;height:900px;"></div>
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
  private mounted() {
    console.log('1');
  }
  private getData() {
    setTimeout((item: any) => {
      this.parData = data.mainData.map((val: any) => {
        console.log(val);
        return [
          val.attributeRatio * 100,
          val.attributeRatio,
          val.eventName,
        ];
      });
      this.parData.unshift(['score', 'amount', 'product']);
      console.log(this.parData);
      this.creatEcharts();
    }, 1000);
  }
  private creatEcharts() {
    const el: any = this.$refs.main;
    this.myChart = Echarts.init(el);
    // 绘制图表
    this.myChart.setOption({
      dataset: {
        source: this.parData,
      },
      grid: { containLabel: true },
      xAxis: { name: 'amount' },
      yAxis: { type: 'category' },
      visualMap: {
        orient: 'horizontal',
        left: 'center',
        min: 10,
        max: 100,
        text: ['High Score', 'Low Score'],
        // Map the score column to color
        dimension: 0,
        inRange: {
          color: ['#D7DA8B', '#E15457'],
        },
      },
      series: [
        {
          type: 'bar',
          encode: {
            // Map the "amount" column to X axis.
            x: 'amount',
            // Map the "product" column to Y axis
            y: 'product',
          },
        },
      ],
    });
  }
}
</script>
<style lang="scss" scoped>
.home {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 400px;
  .box1 {
    height: 100%;
    overflow: auto;
    flex: 1;
  }
  .box2 {
    height: 100%;
    overflow: auto;
    flex: 1;
  }
}
</style>
