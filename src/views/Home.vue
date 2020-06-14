<template>
  <div class="home">
    <button @click="getData">查询</button>
    <button @click="getData1">查询2</button>
    <button @click="getData2">查询3</button>
    <div class="box1">
      <Main v-if="dis" ref="main" />
      <Mains v-else ref="mains" />
    </div>
    <div class="box2">
      <Ring ref="ring"></Ring>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop, Emit, Watch } from 'vue-property-decorator';
import Main from '../components/main.vue';
import Mains from '../components/mains.vue';
import Ring from '../components/ring.vue';
import Echarts from 'echarts';
@Component({
  components: {
    Main, Mains, Ring,
  },
})
export default class Filtercondition extends Vue {
  private dom: any;
  private myChart: any;
  private dis: any = true;
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
    // this.creatEcharts();
  }
  private getData() {
    // console.log(this.$refs.main);
    this.dis = true;
    this.$nextTick(() => {
      (this.$refs.main as any).getData();
    });

  }
  private getData1() {
    this.dis = false;
    this.$nextTick(() => {
      (this.$refs.mains as any).getData();
    });
  }
  private getData2() {
    this.$nextTick(() => {
      (this.$refs.ring as any).getData();
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
