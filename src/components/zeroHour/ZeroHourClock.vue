<template>
  <div
    class="zeroHourClock"
  >
    <div
      class="clockWrapper"
    >
      <div
        class="clock"
      >
        <canvas
          ref="leftZeroClock"
        />
        <canvas
          ref="leftZeroClockOver"
          class="overCanvas"
          @mousemove="onMousemove($event, 'left')"
          @click="onClick($event, 'left')"
        />
      </div>
      <div
        class="clock"
      >
        <canvas
          ref="rightZeroClock"
        />
        <canvas
          ref="rightZeroClockOver"
          class="overCanvas"
          @mousemove="onMousemove($event, 'right')"
          @click="onClick($event, 'right')"
        />
      </div>
    </div>
    <br/>
    <b-button
      @click="resetNum"
    >
      Reset
    </b-button>
    <b-modal
      :active.sync="bothNumberValid"
      has-modal-card
      @close="closeMadal"
    >
      <div
        v-if="getSeqResult.length > 0"
        class="modalWrapper"
      >
        <b-button
          v-for="list in getSeqResult"
          class="cell"
          :key="list.seqResult"
          :class="cellRenderer(list.seqResult)"
        >
          {{ list.seqResult }} ({{ list.console2 }})
        </b-button>
      </div>
      <div
        v-else
        class="modalWrapper"
      >
        <b-button>NO MATCH</b-button>
      </div>
      <br/>
    </b-modal>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
import zeroHourClock from '@/assets/zeroHour/clock.png';

export default {
  name: 'ZeroHourClock',
  components: {
  },
  props: {
    elements: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      // canvas 관련 data
      leftCanvas: null,
      leftCtx: null,
      leftOver: null,
      leftOverCtx: null,
      rightCanvas: null,
      rightOver: null,
      rightCtx: null,
      rightOverCtx: null,
      image: null,
      // canvas size
      canvasSize: 230,
      innerRadius: 60,
      outerRadius: 77,
      // 클릭된 버튼 관련 data
      leftNum: 0, // 왼쪽 선택된 숫자
      rightNum: 0, // 오른쪽 선택된 숫자
      showModal: false,
    };
  },
  mounted() {
    this.timeout = setTimeout(() => {
      this.initCanvas('left');
      this.initCanvas('right');
      this.image = new Image();
      this.image.src = zeroHourClock;
      this.image.onload = () => this.loadClockImage();
    });
  },
  computed: {
    ...mapGetters('ZeroHour', [
      'seqResult',
    ]),
    bothNumberValid: {
      get() {
        return this.leftNum !== 0 && this.rightNum !== 0;
      },
      set(flag) {
        this.showModal = flag;
      },
    },
    getSeqResult() {
      if (this.leftNum !== 0 && this.rightNum !== 0) {
        const lpadLeftNum = String(this.leftNum).length < 2 ? `0${this.leftNum}` : String(this.leftNum);
        const lpadRightNum = String(this.rightNum).length < 2 ? `0${this.rightNum}` : String(this.rightNum);
        return this.seqResult({
          element: this.elements,
          left: lpadLeftNum,
          right: lpadRightNum,
        });
      }
      return false;
    },
  },
  watch: {
    leftNum(num) {
      this.drawArc('left', num);
    },
    rightNum(num) {
      this.drawArc('right', num);
    },
  },
  methods: {
    initCanvas(type) {
      this[`${type}Canvas`] = this.$refs[`${type}ZeroClock`];
      this[`${type}Ctx`] = this[`${type}Canvas`].getContext('2d');
      this[`${type}Canvas`].width = this.canvasSize;
      this[`${type}Canvas`].height = this.canvasSize;
      this[`${type}Canvas`].setAttribute('style', 'position: absolute; top: 0; left: 0;');
      this[`${type}Over`] = this.$refs[`${type}ZeroClockOver`];
      this[`${type}OverCtx`] = this[`${type}Over`].getContext('2d');
      this[`${type}Over`].width = this.canvasSize;
      this[`${type}Over`].height = this.canvasSize;
      this[`${type}Over`].setAttribute('style', 'position: absolute; top: 0; left: 0;');
    },
    loadClockImage() {
      const typeList = ['left', 'right'];
      for (let ix = 0, ixLen = typeList.length; ix < ixLen; ix += 1) {
        const type = typeList[ix];
        const ctx = this[`${type}Ctx`];
        const size = this.canvasSize;
        const inner = this.innerRadius;
        const outer = this.outerRadius;
        ctx.drawImage(this.image, 0, 0);
        ctx.beginPath();
        ctx.arc(
          size / 2, size / 2, inner,
          0, Math.PI * 2,
        );
        ctx.strokeStyle = '#f00';
        ctx.stroke();
        ctx.closePath();
        ctx.beginPath();
        ctx.arc(
          size / 2, size / 2, outer,
          0, Math.PI * 2,
        );
        ctx.strokeStyle = '#f00';
        ctx.stroke();
        ctx.closePath();
      }
    },
    onMousemove(e, type) {
      const mouseX = e.offsetX;
      const mouseY = e.offsetY;
      const centerX = this.canvasSize / 2;
      const centerY = this.canvasSize / 2;
      const diffX = mouseX - centerX;
      const diffY = mouseY - centerY;
      const inner = this.innerRadius;
      const outer = this.outerRadius;
      let pointerOn = false;
      if ((diffX ** 2) + (diffY ** 2) >= (inner ** 2)
        && (diffX ** 2) + (diffY ** 2) <= (outer ** 2)) {
        pointerOn = true;
      }

      if (pointerOn) {
        this[`${type}Over`].style.cursor = 'pointer';
      } else {
        this[`${type}Over`].style.cursor = 'default';
      }
    },
    onClick(e, type) {
      const mouseX = e.offsetX;
      const mouseY = e.offsetY;
      const centerX = this.canvasSize / 2;
      const centerY = this.canvasSize / 2;
      const diffX = mouseX - centerX;
      const diffY = mouseY - centerY;
      let radian = 0;
      const clockRadius = 1 / 6;
      radian = Math.atan2(diffY, diffX) / Math.PI;
      for (let ix = 0, ixLen = 6; ix < ixLen; ix += 1) {
        if (radian >= clockRadius * ix
          && radian < clockRadius * (ix + 1)) {
          this[`${type}Num`] = ix + 4;
          return;
        }
      }
      for (let ix = 0, ixLen = 6; ix < ixLen; ix += 1) {
        if (radian < -1 * clockRadius * ix
          && radian >= -1 * clockRadius * (ix + 1)) {
          if (ix < 3) {
            this[`${type}Num`] = 3 - ix;
            return;
          }
          this[`${type}Num`] = 15 - ix;
          return;
        }
      }
    },
    drawArc(type, num) {
      const ctx = this[`${type}OverCtx`];
      const size = this.canvasSize;
      const inner = this.innerRadius;
      const outer = this.outerRadius;
      const initRadius = -1 * (1 / 2);
      const clockRadius = 1 / 6;
      ctx.clearRect(0, 0, size, size);
      if (num > 0 && num <= 12) {
        ctx.beginPath();
        ctx.arc(
          size / 2, size / 2, inner,
          Math.PI * (initRadius + (clockRadius * (num - 1))),
          Math.PI * (initRadius + (clockRadius * num)),
        );
        ctx.lineTo(
          (size / 2) + (outer * Math.cos(Math.PI * (initRadius + (clockRadius * num)))),
          (size / 2) + (outer * Math.sin(Math.PI * (initRadius + (clockRadius * num)))),
        );
        ctx.arc(
          size / 2, size / 2, outer,
          Math.PI * (initRadius + (clockRadius * num)),
          Math.PI * (initRadius + (clockRadius * (num - 1))),
          true,
        );
        ctx.lineTo(
          (size / 2) + (inner * Math.cos(Math.PI * (initRadius + (clockRadius * (num - 1))))),
          (size / 2) + (inner * Math.sin(Math.PI * (initRadius + (clockRadius * (num - 1))))),
        );
        ctx.fillStyle = '#ff0';
        ctx.fill();
        ctx.closePath();
      }
    },
    resetNum() {
      this.leftNum = 0;
      this.rightNum = 0;
    },
    closeMadal() {
      this.resetNum();
    },
    cellRenderer(seqResult) {
      if (!seqResult) {
        return;
      }
      const color = seqResult.substring(0, 2);
      let resultCls = '';
      if (color === '파랑') {
        resultCls = 'blue';
      } else if (color === '하늘') {
        resultCls = 'sky';
      } else if (color === '빨강') {
        resultCls = 'red';
      } else if (color === '노랑') {
        resultCls = 'yellow';
      } else if (color === '초록') {
        resultCls = 'green';
      } else if (color === '보라') {
        resultCls = 'purple';
      }
      // eslint-disable-next-line
      return resultCls;
    },
  },
  beforeDestroy() {
    clearTimeout(this.timeout);
  },
};
</script>

<style>
  .clockWrapper {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
  }
  .clock {
    position: relative;
    width: 230px;
    height: 230px;
  }
  .overCanvas {
    -webkit-touch-callout: none;
    -webkit-tap-highlight-color: rgba(0,0,0,0);
  }
  .modalWrapper {
    display: flex;
    justify-content: center;
  }
  .cell.blue {
    background-color: blue;
    color: white;
  }
  .cell.red {
    background-color: red;
    color: white;
  }
  .cell.sky {
    background-color: deepskyblue;
    color: white;
  }
  .cell.yellow {
    background-color: yellow;
  }
  .cell.green {
    background-color: green;
    color: white;
  }
  .cell.purple {
    background-color: purple;
    color: white;
  }
</style>
