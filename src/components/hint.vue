<template>
  <div id="hint">{{ HintMessage }}</div>
</template>

<script>
export default {
  name: 'CounterHint',
  props: ['Hour', 'Minute', 'Second', 'hstate', 'hisUp'],
  computed: {
    HintMessage: function () {
      let TimeWord = this.Hour + ':' + this.Minute + ':' + this.Second;
      let message = '';
      switch (this.hstate) {
        case 1:
          if (this.hisUp === 1) {
            message = '正在正计时' + TimeWord;
          } else {
            message = '正在倒计时' + TimeWord;
          }
          break;
        case 2:
          if (this.hisUp === 1) {
            message = '暂停正计时' + TimeWord;
          } else {
            message = '暂停倒计时' + TimeWord;
          }
          break;
        case 3:
          if (this.hisUp === 1) {
            message = '正计时' + TimeWord + '已结束';
          } else {
            message = '倒计时' + TimeWord + '已结束';
          }
          break;
      }
      return message;
    }
  }
}
</script>

<style scoped>
#hint {
    height: 22px;
    font-size: 16px;
    font-family: PTMono-Regular, "PT Mono", PingFangSC-Regular, "PingFang SC", sans-serif;    color: #FFFFFF;
    margin-top: 24px;
    margin-right: 20px;
    display: inline-block;
    float: left;
}
</style>
