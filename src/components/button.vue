<template>
  <div id="button">{{ title }}</div>
</template>

<script>
export default {
  name: 'CounterButton',
  props: ['title']
}
</script>

<style scoped>
#button {
  width: 80px;
  height: 22px;
  box-shadow: 0px 2px 4px 0px rgba(0,0,0,0.10);
  font-size: 16px;
  font-family: PingFangSC-Regular, "PingFang SC", sans-serif;
  color: #FFFFFF;
  margin-top: 15px;
  padding: 9px;
  border-radius: 5px;
  display: inline-block;
  cursor: pointer;
}
</style>
