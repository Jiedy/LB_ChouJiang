<template>
  <div class="main">
    <div class="set-param">
      <div
        style="
          display: flex;
          align-items: center;
          justify-content: center;
          margin-top: 20px;
        "
      >
        抽奖模式：
        <el-radio-group
          v-model="luckDrawMode"
          @change="luckDrawModeChange"
          :disabled="!runGiftState"
        >
          <el-radio label="1">模式1</el-radio>
          <el-radio label="2">模式2</el-radio>
          <el-radio label="3">模式3</el-radio>
        </el-radio-group>
      </div>
      <el-card class="winning-rst">
        <template #header>
          <div class="card-header">
            <span>中奖结果</span>
            <el-popover
              placement="bottom"
              :width="160"
              :visible="visibleClearAll"
            >
              <p>确定清空中奖结果吗？</p>
              <div style="text-align: right; margin: 0">
                <el-button
                  size="mini"
                  type="text"
                  @click="visibleClearAll = false"
                  >取消</el-button
                >
                <el-button type="primary" size="mini" @click="clearWinningRst"
                  >确定</el-button
                >
              </div>
              <template #reference>
                <el-button type="text" @click="visibleClearAll = true"
                  >清空</el-button
                >
              </template>
            </el-popover>
          </div>
        </template>
        <div
          v-for="(value, index) in winningResult"
          :key="index"
          style="margin: 20px"
        >
          <el-popover
            placement="bottom"
            :width="160"
            :visible="visibleClearItem && index == currentPressIndex"
          >
            <p>确定删除该中奖结果吗？</p>
            <div style="text-align: right; margin: 0">
              <el-button
                size="mini"
                type="text"
                @click="visibleClearItem = false"
                >取消</el-button
              >
              <el-button
                type="primary"
                size="mini"
                @click="deleteWinningItem(index)"
                >确定</el-button
              >
            </div>
            <template #reference>
              <div class="winning-text">
                <div style="font-size: 50px; color: #009fff">{{ value }}</div>
                <div
                  class="el-icon-close"
                  style="color: #009fff"
                  @click="
                    visibleClearItem = true;
                    currentPressIndex = index;
                  "
                ></div>
              </div>
            </template>
          </el-popover>
        </div>
      </el-card>
    </div>
    <div
      v-if="luckDrawMode == 1"
      class="luck-draw-area"
      style="flex-direction: column"
    >
      <div
        :style="{
          width: luckdrawW / 1.2 + 'px',
          height: luckdrawH * 0.3 + 'px',
          border: '2px solid gray',
          'border-radius': '10px',
          'font-size': '22vh',
          'box-shadow': '0 15px 20px rgba(0, 0, 0, 0.3) inset',
        }"
        class="roller_container"
        id="machine"
      >
        <div class="roller_container_inner">
          <ul>
            <li
              v-for="(value, index) in liList"
              :key="index"
              :style="{
                'user-select': 'none',
                height: luckdrawH * 0.3 + 'px',
              }"
            >
              {{ value }}
            </li>
          </ul>
        </div>
      </div>
      <div
        :style="{
          'margin-top': '20px',
          display: 'flex',
          width: luckdrawW / 1.2 + 'px',
          'align-items': 'center',
          height: '100px',
        }"
      >
        <el-popover
          placement="right"
          :width="400"
          :visible="setGiftMaxNumberVisible"
        >
          <template #reference>
            <el-button @click="setGiftMaxNumber" :disabled="!runGiftState"
              >设置最大编号</el-button
            >
          </template>
          <div class="flex-row">
            <div>请输入最大编号：</div>
            <el-input-number
              v-model="giftMaxNumber"
              controls-position="right"
              :min="1"
              :max="999"
              ref="giftMaxNumber"
              @keyup.enter="setGiftMaxNumberVisible = false"
            ></el-input-number>
            <el-button @click="setGiftMaxNumberVisible = false">确定</el-button>
          </div>
        </el-popover>
        <el-button
          style="
            font-size: 4vw;
            position: absolute;
            left: 50%;
            transform: translate(-50%, 0);
          "
          type="info"
          @click="runGift0"
          :disabled="!runGiftState"
          >启动</el-button
        >
      </div>
    </div>
    <div
      v-if="luckDrawMode == 2"
      class="luck-draw-area"
      style="flex-direction: column"
    >
      <div
        :style="{
          width: luckdrawW / 1.2 + 'px',
          height: luckdrawH * 0.3 + 'px',
          border: '2px solid gray',
          'border-radius': '10px',
          'font-size': '8vw',
        }"
        class="container"
      >
        <Gift
          v-for="(config, index) in configs"
          :config="config"
          :key="index"
          :giftIndex="index"
          @finished="finished"
          :ref="'gift' + index"
          :giftClass="
            index == 0 ? giftClass1 : index == 1 ? giftClass2 : giftClass3
          "
        >
        </Gift>
      </div>
      <div
        :style="{
          'margin-top': '20px',
          display: 'flex',
          width: luckdrawW / 1.2 + 'px',
          'align-items': 'center',
          height: '100px',
        }"
      >
        <el-popover
          placement="right"
          :width="400"
          :visible="setGiftMaxNumberVisible"
        >
          <template #reference>
            <el-button @click="setGiftMaxNumber" :disabled="!runGiftState"
              >设置最大编号</el-button
            >
          </template>
          <div class="flex-row">
            <div>请输入最大编号：</div>
            <el-input-number
              v-model="giftMaxNumber"
              controls-position="right"
              :min="1"
              :max="999"
              ref="giftMaxNumber"
              @keyup.enter="setGiftMaxNumberVisible = false"
            ></el-input-number>
            <el-button @click="setGiftMaxNumberVisible = false">确定</el-button>
          </div>
        </el-popover>
        <el-button
          style="
            font-size: 4vw;
            position: absolute;
            left: 50%;
            transform: translate(-50%, 0);
          "
          type="info"
          @click="runGift"
          :disabled="!runGiftState"
          >启动</el-button
        >
      </div>
    </div>
    <div v-else-if="luckDrawMode == 3" class="luck-draw-area">
      <div
        :style="{
          'user-select': 'none',
          width: luckdrawPx * 0.23 + 'px',
          height: luckdrawPx * 0.23 + 'px',
          'border-radius': '50%',
          'background-color': 'orange',
          'z-index': 1,
          display: 'flex',
          'align-items': 'center',
          'justify-content': 'center',
          'font-size': '3.5vw',
          'white-space': 'pre',
        }"
        :class="{ 'btn-start': btnStartEnable, 'txt-disable': !btnStartEnable }"
        @mouseover="mouseoverBtnStart(false)"
        @mouseout="mouseoverBtnStart(false)"
        @click="btnStart"
      >
        {{ gameState }}
      </div>
      <div
        v-for="cycle in [1, 2, 3]"
        :key="cycle"
        style="position: absolute"
        :class="{
          pulse: pulse,
          blockAnimation: blockAnimation,
          blockAnimation_no: blockAnimation_no,
          blockAnimation_stopping_1: cycle == 1 && blockAnimation_stopping_1,
          blockAnimation_stopping_2: cycle == 2 && blockAnimation_stopping_2,
          blockAnimation_stopping_3: cycle == 3 && blockAnimation_stopping_3,
        }"
      >
        <div
          :style="{
            width: luckdrawPx * (0.95 - 0.24 * cycle) + 'px',
            height: luckdrawPx * (0.95 - 0.24 * cycle) + 'px',
            border: luckdrawPx * 0.12 + 'px solid ' + color_tmp[cycle - 1],
            'border-radius': '50%',
          }"
          :id="'luckdraw' + cycle"
        >
          <div
            v-for="(worker, index) in workerInfoSp[cycle - 1]"
            :key="worker"
            :style="{
              'user-select': 'none',
              position: 'absolute',
              top: (luckdrawPx * 0.12) / 2 + 'px',
              left: '50%',
              transform:
                'rotate(' +
                (360 * index) / workerInfoSp[cycle - 1].length +
                'deg) translate(-50%, -50%)',
              'transform-origin':
                '0 ' +
                ((luckdrawPx * (0.95 - 0.24 * cycle)) / 2 +
                  (luckdrawPx * 0.12) / 2) +
                'px',
            }"
          >
            <div style="transform: rotate(-90deg)">
              {{ convertNumToStr(worker.num + 1) }}
            </div>
          </div>
        </div>
      </div>
      <hr
        :style="{
          width: (luckdrawPx - luckdrawPx * (0.95 - 0.24 * 3)) / 2 + 'px',
          position: 'absolute',
          left: (luckdrawW + luckdrawPx * (0.95 - 0.24 * 3)) / 2 + 'px',
          top: luckdrawH / 2 + 'px',
        }"
      />
      <div class="set-maxnum">
        <el-popover
          placement="right"
          :width="400"
          :visible="setGiftMaxNumberVisible"
        >
          <template #reference>
            <el-button @click="setGiftMaxNumber" :disabled="!runGiftState"
              >设置最大编号</el-button
            >
          </template>
          <div class="flex-row">
            <div>请输入最大编号：</div>
            <el-tooltip
              content="此模式最大值为：200"
              placement="top"
              effect="light"
              v-model="setGiftMaxNumberVisible"
              :manual="true"
            >
              <el-input-number
                v-model="giftMaxNumber"
                controls-position="right"
                :min="1"
                :max="999"
                ref="giftMaxNumber"
                @keyup.enter="setGiftMaxNumberVisible = false"
              ></el-input-number>
            </el-tooltip>
            <el-button @click="setGiftMaxNumberVisible = false">确定</el-button>
          </div>
        </el-popover>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent } from "vue";
import $ from "jquery";
import Gift from "./Gift.vue";
import { TIMEOUT } from "node:dns";

export default defineComponent({
  name: "HelloWorld",
  components: {
    Gift,
  },
  props: {},
  setup: () => {},
  watch: {
    giftMaxNumber(newVar: number, oldVar: number) {
      this.reSetGiftMaxNumber();
      this.initLi();
      this.initLuckdraw();
    },
  },
  beforeMount() {
    this.configs = [
      {
        duration: 4000,
        gifts: Array.from(new Array(10), (val, index) => {
          return { type: "text", name: index };
        }),
      },
      {
        duration: 4000,
        gifts: Array.from(new Array(10), (val, index) => {
          return { type: "text", name: index };
        }),
      },
      {
        duration: 4000,
        gifts: Array.from(new Array(10), (val, index) => {
          return { type: "text", name: index };
        }),
      },
    ];
  },
  data: () => {
    return {
      configs: null as any,
      luckDrawMode: "1",
      gameState: "开始",
      btnStartEnable: true,
      pulse: false,
      luckdrawAutoStop: null as any,
      blockAnimation: false,
      blockAnimation_no: true,
      blockAnimation_stopping_1: false,
      blockAnimation_stopping_2: false,
      blockAnimation_stopping_3: false,
      workerInfo: [{ num: 1, name: "" }],
      workerInfoSp: [[{ num: 1, name: "" }]],
      workerInfoRst: new Array(),
      luckdrawW: 0,
      luckdrawH: 0,
      luckdrawPx: 0,
      color_tmp: [
        "rgba(0, 255, 255, 0.5)",
        "rgba(211, 211, 211, 0.5)",
        "rgba(153, 205, 50, 0.5)",
      ],
      mouseInBtnStart: false,
      runGiftState: true,
      giftMaxNumber: 999,
      setGiftMaxNumberVisible: false,
      giftNumberStr: "",
      giftClass1: "bg-start",
      giftClass2: "bg-start",
      giftClass3: "bg-start",
      liList: ["0 0 1", "0 0 2", "0 0 3"],
      liCount: 0,
      liHeights: new Array(),
      liStops: null as any,
      winningResult: new Array<String>(),
      visibleClearAll: false,
      visibleClearItem: false,
      currentPressIndex: 0,
    };
  },
  methods: {
    deleteWinningItem(index: number) {
      this.visibleClearItem = false;
      this.winningResult.splice(index, 1);
    },
    clearWinningRst() {
      this.visibleClearAll = false;
      this.winningResult = new Array<string>();
    },
    shuffle(array: Array<any>) {
      var m = array.length,
        t,
        i;
      while (m) {
        i = Math.floor(Math.random() * m--);
        t = array[m];
        array[m] = array[i];
        array[i] = t;
      }
      return array;
    },
    mouseoverBtnStart(isIn: boolean) {
      this.mouseInBtnStart = isIn;
    },
    setGiftMaxNumber() {
      this.setGiftMaxNumberVisible = !this.setGiftMaxNumberVisible;
      this.$nextTick(() => {
        // (this.$refs.giftMaxNumber as any).$el.children[2].children[0].focus();
        (this.$refs.giftMaxNumber as any).input.focus();
      });
    },
    convertNumToStr(index: number) {
      if (index < 10) return "0 0 " + index;
      else if (index < 100)
        return "0 " + parseInt(index / 10 + "") + " " + (index % 10);
      else if (index < 1000)
        return (
          parseInt(index / 100 + "") +
          " " +
          (parseInt(index / 10 + "") % 10) +
          " " +
          (index % 10)
        );
      else return "" + index;
    },
    async runGift() {
      this.reSetGiftMaxNumber();
      await this.$nextTick();
      (this.$refs.gift0 as any).autoTurn(this.configs[0].duration);
      this.runGiftState = false;
    },
    async initLi() {
      this.liList = Array.from(
        this.shuffle(
          Array.from(new Array(this.giftMaxNumber), (val, index) => index + 1)
        ),
        (index) => this.convertNumToStr(index)
      );
      await this.$nextTick();
      const that = this;
      var machine = $("#machine");
      that.liCount = 0;
      machine.find("ul").each(function () {
        $(this).css("top", 0);
        var liheight = $(this).find("li:eq(0)").height();
        that.liHeights[that.liCount++] =
          $(this).find("li").length * (liheight ?? 0);
      });
    },
    async runGift0() {
      this.runGiftState = false;
      await this.initLi();
      // shake the things
      this.liCount = 0;
      const machine = $("#machine");
      const that = this;
      this.liStops = [];
      machine.find("ul").each(function () {
        that.liStops[that.liCount] = false;
        setTimeout(function () {
          that.liStops[that.liCount++] = true;
        }, 4000);
        that.roll($(this), that.liCount++);
      });
      that.liCount = 0;
    },
    roll(ul: any, count: number) {
      const that = this;
      ul.animate(
        { top: "-" + this.liHeights[count] + "px" },
        200,
        "linear",
        function () {
          ul.css("top", 0);
          if (!that.liStops[count]) {
            that.roll(ul, count);
          } else {
            // ease to a stop
            var num = Math.floor(Math.random() * (ul.find("li").length / 2));
            var liheight = ul.find("li:eq(0)").height();
            liheight = liheight * num;
            ul.animate({ top: "-" + liheight + "px" }, 10, "linear", () => {
              that.winningResult.push(that.liList[num]);
              that.runGiftState = true;
            });
          }
        }
      );
    },
    reSetGiftMaxNumber() {
      let configsCopy = JSON.parse(JSON.stringify(this.configs));
      if (this.giftMaxNumber < 100) {
        configsCopy[0].gifts = [{ type: "text", name: 0 }];
        configsCopy[0].duration = 0;
      } else {
        configsCopy[0].gifts = Array.from(
          new Array(1 + parseInt(this.giftMaxNumber / 100 + "")),
          (val, index) => {
            return { type: "text", name: index };
          }
        );
        configsCopy[0].duration = 4000;
      }
      if (this.giftMaxNumber < 10) {
        configsCopy[1].gifts = [{ type: "text", name: 0 }];
        configsCopy[1].duration = 0;
        configsCopy[2].gifts = Array.from(
          new Array(1 + this.giftMaxNumber),
          (val, index) => {
            return { type: "text", name: index };
          }
        );
      } else {
        configsCopy[1].gifts = Array.from(new Array(10), (val, index) => {
          return { type: "text", name: index };
        });
        configsCopy[1].duration = 5000;
      }
      this.configs = configsCopy;
      this.giftClass1 = "bg-start";
      this.giftClass2 = "bg-start";
      this.giftClass3 = "bg-start";
    },
    finished(param: any) {
      if (param.index == 0) {
        this.giftClass1 = "bg-end";
        if (this.giftMaxNumber < 100) this.giftNumberStr = "0";
        else this.giftNumberStr = param.text + "";
        if (
          this.giftNumberStr ==
          parseInt(this.giftMaxNumber / 100 + "") + ""
        ) {
          let shiwei = parseInt((this.giftMaxNumber % 100) / 10 + "");
          this.configs[1].gifts = Array.from(
            new Array(1 + shiwei),
            (val, index) => {
              return { type: "text", name: index };
            }
          );
        } else
          this.configs[1].gifts = Array.from(new Array(10), (val, index) => {
            return { type: "text", name: index };
          });
        this.$nextTick(() => {
          (this.$refs.gift1 as any).autoTurn(this.configs[1].duration);
        });
      }
      if (param.index == 1) {
        this.giftClass2 = "bg-end";
        this.giftNumberStr += param.text;
        let gewei = this.giftMaxNumber % 10;
        let baishi = (this.giftMaxNumber - gewei) / 10;
        if (baishi == parseInt(this.giftNumberStr)) {
          this.configs[2].gifts = Array.from(
            new Array(1 + gewei),
            (val, index) => {
              return { type: "text", name: index };
            }
          );
        } else
          this.configs[2].gifts = Array.from(new Array(10), (val, index) => {
            return { type: "text", name: index };
          });
        this.$nextTick(() => {
          (this.$refs.gift2 as any).autoTurn(this.configs[2].duration);
        });
      }
      if (param.index == 2) {
        this.giftClass3 = "bg-end";
        this.giftNumberStr += param.text;
        this.winningResult.push(
          this.convertNumToStr(Number(this.giftNumberStr))
        );
        this.runGiftState = true;
      }
    },
    luckDrawModeChange(val: any) {
      this.setGiftMaxNumberVisible = false;
      if (val == 2) {
        this.giftClass1 = "bg-start";
        this.giftClass2 = "bg-start";
        this.giftClass3 = "bg-start";
      }
    },
    btnStart() {
      if (!this.btnStartEnable) return;
      if (this.luckdrawAutoStop != null) clearTimeout(this.luckdrawAutoStop);
      switch (this.gameState) {
        case "开始":
          this.gameState = "停";
          this.luckdrawAutoStop = setTimeout(() => {
            this.btnStart();
          }, 5000);
          break;
        case "停":
          this.gameState = "准备\n结果";
          this.btnStartEnable = false;
          this.setSpinningStoppingDeg();
          setTimeout(() => {
            this.btnStartEnable = true;
            this.gameState = "恭喜\n中奖";
          }, 8000);
          break;
        case "恭喜\n中奖":
          this.gameState = "开始";
          break;
      }
      this.pulse = this.mouseInBtnStart;
      this.blockAnimation = this.gameState == "停";
      this.blockAnimation_no = this.gameState == "开始";
      this.blockAnimation_stopping_1 = this.blockAnimation_stopping_2 = this.blockAnimation_stopping_3 =
        this.gameState == "准备\n结果";
    },
    setSpinningStoppingDeg() {
      let matrixStr = $(".blockAnimation").css("transform");
      let values = matrixStr.split("(")[1].split(")")[0].split(",");
      let a = Number(values[0]);
      let b = Number(values[1]);
      let c = Number(values[2]);
      let d = Number(values[3]);
      let e = Number(values[4]);
      let f = Number(values[5]);
      let deg = this.getmatrix(a, b, c, d, e, f);

      let addDeg =
        360 / this.workerInfoSp[0].length -
        ((360 + deg) % (360 / this.workerInfoSp[0].length));
      const kfs = this.findKeyframesRule("spinning-stopping-1");
      if (kfs != null) kfs.ss.deleteRule(kfs.index);
      kfs?.ss.insertRule(
        `@keyframes spinning-stopping-1 {
  0% {
    transform: rotate(` +
          deg +
          `deg);
  }
  100% {
    transform: rotate(` +
          (360 + deg + addDeg + 90) +
          `deg);
  }
}`
      );

      let addDeg2 =
        360 / this.workerInfoSp[1].length -
        ((360 + deg) % (360 / this.workerInfoSp[1].length));
      const kfs2 = this.findKeyframesRule("spinning-stopping-2");
      if (kfs2 != null) kfs2.ss.deleteRule(kfs2.index);
      kfs2?.ss.insertRule(
        `@keyframes spinning-stopping-2 {
  0% {
    transform: rotate(` +
          deg +
          `deg);
  }
  100% {
    transform: rotate(` +
          (360 + deg + addDeg2 + 90) +
          `deg);
  }
}`
      );

      let addDeg3 =
        360 / this.workerInfoSp[2].length -
        ((360 + deg) % (360 / this.workerInfoSp[2].length));
      const kfs3 = this.findKeyframesRule("spinning-stopping-3");
      if (kfs3 != null) kfs3.ss.deleteRule(kfs3.index);
      kfs3?.ss.insertRule(
        `@keyframes spinning-stopping-3 {
  0% {
    transform: rotate(` +
          deg +
          `deg);
  }
  100% {
    transform: rotate(` +
          (360 + deg + addDeg3 + 90) +
          `deg);
  }
}`
      );
      let degRst = (((deg + addDeg) % 360) + 90) % 360;
      setTimeout(() => {
        let sortList = [];
        for (let element of $("#luckdraw1").children()) {
          let itemDeg = parseInt(
            element.style.transform.split("(")[1].split("deg")[0]
          );
          sortList.push({
            order: Math.abs(itemDeg + degRst - 90 - 360) % 360,
            el: element,
          });
        }
        sortList.sort((a, b) => a.order - b.order);
        this.winningResult.push(sortList[0].el.innerText);
      }, 8000);
      let degRst2 = (((deg + addDeg2) % 360) + 90) % 360;
      setTimeout(() => {
        let sortList = [];
        for (let element of $("#luckdraw2").children()) {
          let itemDeg = parseInt(
            element.style.transform.split("(")[1].split("deg")[0]
          );
          sortList.push({
            order: Math.abs(itemDeg + degRst2 - 90 - 360) % 360,
            el: element,
          });
        }
        sortList.sort((a, b) => a.order - b.order);
        this.winningResult.push(sortList[0].el.innerText);
      }, 6000);
      let degRst3 = (((deg + addDeg3) % 360) + 90) % 360;
      setTimeout(() => {
        let sortList = [];
        for (let element of $("#luckdraw3").children()) {
          let itemDeg = parseInt(
            element.style.transform.split("(")[1].split("deg")[0]
          );
          sortList.push({
            order: Math.abs(itemDeg + degRst3 - 90 - 360) % 360,
            el: element,
          });
        }
        sortList.sort((a, b) => a.order - b.order);
        this.winningResult.push(sortList[0].el.innerText);
      }, 4000);
    },
    findKeyframesRule(rule: string) {
      //此处过滤非同源的styleSheet，因为非同源的无法访问cssRules，会报错
      var ss = Array.from(document.styleSheets).filter(
        (styleSheet) =>
          !styleSheet.href || styleSheet.href.startsWith(window.location.origin)
      );
      for (var i = 0; i < ss.length; ++i) {
        for (var j = 0; j < ss[i].cssRules.length; ++j) {
          if (
            ss[i].cssRules[j].type === window.CSSRule.KEYFRAMES_RULE &&
            (ss[i].cssRules[j] as CSSKeyframesRule).name === rule
          ) {
            return { ss: ss[i], index: j };
          }
        }
      }
      return null;
    },
    getmatrix(
      a: number,
      b: number,
      c: number,
      d: number,
      e: number,
      f: number
    ) {
      var aa = Math.round((180 * Math.asin(a)) / Math.PI);
      var bb = Math.round((180 * Math.acos(b)) / Math.PI);
      var cc = Math.round((180 * Math.asin(c)) / Math.PI);
      var dd = Math.round((180 * Math.acos(d)) / Math.PI);
      var deg = 0;
      if (aa == bb || -aa == bb) {
        deg = dd;
      } else if (-aa + bb == 180) {
        deg = 180 + cc;
      } else if (aa + bb == 180) {
        deg = 360 - cc || 360 - dd;
      }
      return deg >= 360 ? 0 : deg;
    },
    initLuckdraw() {
      this.workerInfo = [];
      this.workerInfoSp = [];
      for (
        let i = 0;
        i < (this.giftMaxNumber > 200 ? 200 : this.giftMaxNumber);
        i++
      ) {
        this.workerInfo.push({ num: i, name: "Name:" + i });
      }
      this.workerInfo = this.shuffle(this.workerInfo);
      //#region 均分
      // for (let cycle of [1, 2, 3]) {
      //   this.workerInfoSp.push(
      //     this.workerInfo.slice(
      //       (cycle - 1) * parseInt(this.workerInfo.length / 3 + ""),
      //       cycle == 3
      //         ? 100000000
      //         : cycle * parseInt(this.workerInfo.length / 3 + "")
      //     )
      //   );
      // }
      //#endregion
      //#region 1.5倍
      let totalP = 1 + 1.5 + 2.25;
      let totalAddCount = 0;
      for (let cycle of [1, 2, 3]) {
        this.workerInfoSp.push(
          this.workerInfo.slice(
            totalAddCount,
            cycle == 3
              ? 100000000
              : parseInt(
                  (Math.pow(1.5, cycle - 1) / totalP) * this.workerInfo.length +
                    totalAddCount +
                    ""
                )
          )
        );
        totalAddCount += this.workerInfoSp[cycle - 1].length;
      }
      //#endregion
      this.workerInfoSp.reverse();
    },
  },
  mounted() {
    this.initLuckdraw();
    let getWH = () => {
      let ldaEl = document.getElementsByClassName(
        "luck-draw-area"
      )[0] as HTMLElement;
      this.luckdrawW = parseInt(window.getComputedStyle(ldaEl, null)["width"]);
      this.luckdrawH = parseInt(window.getComputedStyle(ldaEl, null)["height"]);
      this.luckdrawPx = Math.min(this.luckdrawW, this.luckdrawH);
      for (let config of this.configs) {
        config.height = this.luckdrawH * 0.3;
        config.width = this.luckdrawH * 0.3;
      }
    };
    getWH();
    window.onresize = () => {
      getWH();
      const that = this;
      var machine = $("#machine");
      machine.find("ul").each(function () {
        var liheight = $(this).find("li:eq(0)").height();
        that.liHeights[that.liCount++] =
          $(this).find("li").length * (liheight ?? 0);
      });
    };
  },
});
</script>

<style>
.main {
  height: 100%;
  display: flex;
}
.main::after {
  background: url("./../assets/bg.png");
  background-size: cover;
  content: "";
  opacity: 0.5;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  position: absolute;
  z-index: -1;
}
.flex-row {
  display: flex;
  align-items: center;
  justify-content: center;
}
.set-param {
  width: 400px;
  display: flex;
  align-items: center;
  flex-direction: column;
  border-right: 1px gray solid;
}
.set-param .el-radio__input.is-checked+.el-radio__label{
  color: #88b9ec !important;
}
.winning-rst {
  margin-top: 20px;
  width: 360px;
}
.winning-rst .el-card{
  background-color:#FF868E !important;
}
.winning-rst .el-button--text{
  color:#8dc5ff !important;
}
.winning-text {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #eaf5ff;
  border: 1px #bfe3ff solid;
  border-radius: 7px;
  padding: 10px;
}
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 10px;
  color: red;
}
.luck-draw-area {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
  cursor: default;
}
.blockAnimation {
  animation: spinning 0.8s infinite linear;
}
.blockAnimation_no {
  animation: spinning 60s infinite linear;
}
.blockAnimation_stopping_1 {
  animation: spinning-stopping-1 8s ease-out;
  animation-fill-mode: forwards;
}
.blockAnimation_stopping_2 {
  animation: spinning-stopping-2 6s ease-out;
  animation-fill-mode: forwards;
}
.blockAnimation_stopping_3 {
  animation: spinning-stopping-3 4s ease-out;
  animation-fill-mode: forwards;
}
.btn-start:hover {
  cursor: pointer;
  text-shadow: 2px 2px 2px #ff0000;
}
.pulse {
  opacity: 0;
  animation: warn 3s ease-out;
  animation-iteration-count: infinite;
}
.txt-disable {
  color: gray;
}

.container {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 15px 20px rgba(0, 0, 0, 0.3) inset;
  overflow: hidden;
}

.bg-start {
  background-color: mistyrose;
}

.bg-end {
  background-color: chartreuse;
}

.set-maxnum {
  position: absolute;
  left: 20px;
  bottom: 20px;
}

@keyframes spinning {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
@keyframes spinning-stopping-1 {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
@keyframes spinning-stopping-2 {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
@keyframes spinning-stopping-3 {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes warn {
  0% {
    transform: scale(0.2);
    opacity: 0;
  }
  20% {
    transform: scale(0.5);
    opacity: 0.4;
  }
  40% {
    transform: scale(0.8);
    opacity: 0.4;
  }
  60% {
    transform: scale(1);
    opacity: 0.8;
  }
  100% {
    transform: scale(1);
    opacity: 0;
  }
}

.roller_container {
  font-family: verdana;
  color: #444;
  border: solid 1px #ccc;
  border-right: none;
  overflow: hidden;
  background-color: #fff;
}
.roller_container ul {
  border-right: solid 1px #ccc;
  overflow: hidden;
  padding: 0px;
  margin: 0px;
  position: relative;
}
.roller_container ul li {
  list-style: none;
  overflow: hidden;
  padding: 0px;
  margin: 0px;
  text-align: center;
  display: block;
}
</style>