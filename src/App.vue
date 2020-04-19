<template>
  <div id="app">
    <body><header>
      <div id="Rectango" height=100%>
        <counter-input title="时" v-model="setHour" v-if="state===0"></counter-input>
        <counter-input title="分" v-model="setMinute" v-if="state===0"></counter-input>
        <counter-input title="秒" v-model="setSecond" v-if="state===0"></counter-input>
        <counter-button title="开始正计时" style="margin-right:20px; background-color:#2188E9"
          @click.native="countUp" v-if="state===0">
        </counter-button>
        <counter-button title="开始倒计时" style="margin-right:20px; background-color:#2188E9"
          @click.native="countDown" v-if="state===0">
        </counter-button>
        <counter-hint :hour="this.setHour" :minute="this.setMinute" :second="this.setSecond"
          :hstate="this.state" :his-up="this.isUp" v-if="state!==0">
        </counter-hint>
        <counter-button title="恢复计时器" style="margin-right:20px; background-color:#2188E9"
          @click.native="resume" v-if="state===2">
        </counter-button>
        <counter-button title="暂停计时器" style="margin-right:20px; background-color:#2188E9"
          @click.native="pause" v-if="state===1">
        </counter-button>
        <counter-button title="重新再计时" style="margin-left:20px; float:right; background-color:#FFB020"
          @click.native="restart" v-if="state!==0">
        </counter-button>
        <counter-button title="清空正计时" style="margin-left:20px; float:right; background-color:#DD2E1D"
          @click.native="clearCount" v-if="state!==0 && isUp===1">
        </counter-button>
        <counter-button title="清空倒计时" style="margin-left:20px; float:right; background-color:#DD2E1D"
          @click.native="clearCount" v-if="state!==0 && isUp===0">
        </counter-button>
      </div>
    </header>
    <div id="Rectango" height=440px>
      <div id="time">{{ checkTime(hr) + ':' + checkTime(min) + ':' + checkTime(sec) }}</div>
    </div>
    </body>
  </div>
</template>

<script>
import CounterButton from '@/components/button.vue'
import CounterInput from '@/components/input.vue'
import CounterHint from '@/components/hint.vue'
export default {
  components: {
    CounterButton,
    CounterInput,
    CounterHint
  },
  data () {
    return {
      move: '',
      isUp: 1,
      state: 0, // 0=counter, 1=counting, 2=paused, 3=counted
      setTime: 0,
      time: 0,
      setHour: 0,
      setMinute: 0,
      setSecond: 0,
      hr: 0,
      min: 0,
      sec: 0
    }
  },
  created: function () {
    let self = this;
    document.onkeydown = function (ev) {
      switch (ev.keyCode) {
        case 108: // enter
        case 13: // enter
          window.event.preventDefault();
          if (self.state === 0) { // counter
            self.countUp();
          }
          break;
        case 32:// space
          window.event.preventDefault();
          if (self.state === 1) { // counting
            self.pause();
          } else if (self.state === 2) { // paused
            self.resume();
          }
          break;
      }
    }
  },
  methods: {
    checkTime: function (num) {
      if (num === 0) {
        num = '00';
      } else if (num < 10) { num = '0' + num; }
      return num;
    },
    moveUp: function () {
      this.time = this.time + 100;
      this.hr = Math.floor(this.time / 3600000);
      if (this.hr < 0) this.hr = 0;
      this.min = Math.floor((this.time - this.hr * 3600000) / 60000);
      if (this.min < 0) this.min = 0;
      this.sec = Math.floor((this.time - this.hr * 3600000 - this.min * 60000) / 1000);
      if (this.sec < 0) this.sec = 0;
      if (this.time >= this.setTime) {
        clearInterval(this.move);
        this.state = 3;
      }
    },
    moveDown: function () {
      this.time = this.time - 100;
      this.hr = Math.floor(this.time / 3600000);
      if (this.hr < 0) this.hr = 0;
      this.min = Math.floor((this.time - this.hr * 3600000) / 60000);
      if (this.min < 0) this.min = 0;
      this.sec = Math.floor((this.time - this.hr * 3600000 - this.min * 60000) / 1000);
      if (this.sec < 0) this.sec = 0;
      if (this.time <= 0) {
        clearInterval(this.move);
        this.state = 3;
      }
    },
    getTime: function () {
      if (this.setHour >= 0 && this.setHour <= 59) {
        this.setHour = Math.floor(this.setHour);
      } else { this.setHour = 0; }
      if (this.setMinute >= 0 && this.setMinute <= 59) {
        this.setMinute = Math.floor(this.setMinute);
      } else if (this.setMinute > 59) {
        this.setMinute = 59;
      } else { this.setMinute = 0; }
      if (this.setSecond >= 0 && this.setSecond <= 59) {
        this.setSecond = Math.floor(this.setSecond);
      } else if (this.setSecond > 59) {
        this.setSecond = 59;
      } else { this.setSecond = 0; }
      this.setTime = this.setHour * 3600000 + this.setMinute * 60000 + this.setSecond * 1000;
      this.setHour = this.checkTime(this.setHour);
      this.setMinute = this.checkTime(this.setMinute);
      this.setSecond = this.checkTime(this.setSecond);
    },
    countUp: function (event) {
      this.state = 1;// counting
      this.isUp = 1;
      this.getTime();
      this.time = 0;
      this.move = setInterval(this.moveUp, 100);
    },
    countDown: function () {
      this.state = 1;// counting
      this.isUp = 0;
      this.getTime();
      this.time = this.setTime;
      this.move = setInterval(this.moveDown, 100);
    },
    resume: function () {
      this.state = 1;// counting
      if (this.isUp === 1) {
        this.move = setInterval(this.moveUp, 100);
      } else { this.move = setInterval(this.moveDown, 100); }
    },
    pause: function () {
      this.state = 2;// paused
      if (this.move !== null) { clearInterval(this.move); }
    },
    restart: function () {
      this.state = 1;// counting
      if (this.move !== null) { clearInterval(this.move); }
      if (this.isUp === 1) {
        this.time = 0;
        this.move = setInterval(this.moveUp, 100);
      } else {
        this.time = this.setTime;
        this.move = setInterval(this.moveDown, 100);
      }
    },
    clearCount: function () {
      this.state = 0;// counter
      if (this.move !== null) { clearInterval(this.move); }
      this.setHour = 0;
      this.setMinute = 0;
      this.setSecond = 0;
      this.hr = 0;
      this.min = 0;
      this.sec = 0;
    }
  }
};
</script>

<style scoped>
body {
    width: 1220px;
    height: 510px;
    background: #F2F4F7;
}
header {
    width: 1220px;
    height: 70px;
    background: #97A5BC;
    box-shadow: 0px 2px 4px 0px rgba(0,0,0,0.10);
}
#Rectango {
    width: 1140px;
    margin-left: 40px;
    margin-right: 40px;
}
#time {
    width: 960px;
    height: 200px;
    margin: 127px 90px 113px 91px;
    font-size: 200px;
    font-family: PTMono-Bold, "PT Mono", monospace;
    font-weight: 700;
    color: #333333;
}
</style>
