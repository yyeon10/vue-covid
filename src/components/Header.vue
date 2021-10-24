<template>
  <div class="wrap">
    <img src="@/assets/covid-icon.png" alt="covid icon" class="icon" />
    <div class="pre">코로나 바이러스 감염증 - 19</div>
    <div class="title">국내 검사 및 확진자 현황</div>
  </div>
</template>

<script>
export default {
  name: "Header",
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.wrap {
  font-family: "GmarketSansMedium";
  text-align: center;
  width: 552px;
}

.pre {
  font-size: 30px;
  letter-spacing: 0.33em;
  border-top: 3px solid #fff;
  border-bottom: 3px solid #fff;
}

.title {
  font-size: 50px;
  font-weight: bold;
}

.icon {
  width: 100px;
}
</style>
