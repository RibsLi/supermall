<template>
  <div class="base-line">
    <div class="line"></div>
    <div class="line-center">已经没有啦</div>
    <div class="line"></div>
  </div>
</template>

<script>
export default {
  name: "DetailBaseLine",
};
</script>

<style scoped>
.base-line {
  position: absolute;
  bottom: -34px;
  left: 0;
  right: 0;
  padding: 10px 0px;
  background-color: #f2f2f2;
  text-align: center;
  font-size: 12px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.line-center {
  padding: 0px 5px;
}
.line {
  display: inline-block;
  width: 20%;
  border-top: 0.01rem solid #999;
}
</style>