<template>
  <div id="input">
    <input id="setTime" placeholder="0" type="text"
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)">
    <div id="Label">{{ " " + title }}</div>
  </div>
</template>

<script>
export default {
  name: 'CounterInput',
  props: ['title', 'value']
}
</script>

<style scoped>
#input {
  height: 22px;
  background: #FFFFFF;
  box-shadow: 0px 2px 4px 0px rgba(0,0,0,0.10);
  margin-right: 20px;
  margin-top: 15px;
  padding: 9px;
  border-radius: 5px;
  display: inline-block;
  box-lines: single;
}
#setTime {
  width: 20px;
  height: 16px;
  border-width: 0;
  font-size: 16px;
  font-family: PingFangSC-Regular, "PingFang SC", sans-serif;
  color: #AFAFAF;
  display: inline-block;
}
input::placeholder {
  color: #AFAFAF;
}
#Label {
  width: 32px;
  height: 22px;
  font-size: 16px;
  font-family: PingFangSC-Regular, "PingFang SC", sans-serif;
  color: #222222;
  display: inline-block;
  text-align: right;
}
</style>
