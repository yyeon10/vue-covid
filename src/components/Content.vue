<template>
  <div class="wrap">
    <div class="item-wrap">
      <div class="item">
        <div class="label">확진자 수</div>
        <div class="data">20000명</div>
      </div>
      <div class="item">
        <div class="label">사망자 수</div>
        <div class="data">20000명</div>
      </div>
      <div class="item">
        <div class="label">치료중 환자 수</div>
        <div class="data">20000명</div>
      </div>
      <div class="item">
        <div class="label">누적 확진률</div>
        <div class="data">20000명</div>
      </div>
    </div>
    <div class="date">
      <span>2021년 10월 24일 20시 기준</span>
      <img src="@/assets/refresh.png" alt="refresh" class="icon" />
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Content",
  methods: {
    getCovidInfo: async () => {
      let url = "/openapi/service/rest/Covid19/getCovid19InfStateJson"; /*URL*/
      let queryParams =
        "?" +
        encodeURIComponent("serviceKey") +
        "=" +
        "GGw58UOnRwx4EftqiTrQ7LFYo6ATZicDZ5H4IG%2F16UAy0m9ExsZys530QgmoUfvQkJQAO4WV71IHRnJpEndp1w%3D%3D"; /*Service Key*/
      queryParams +=
        "&" + encodeURIComponent("pageNo") + "=" + encodeURIComponent("1"); /**/
      queryParams +=
        "&" +
        encodeURIComponent("numOfRows") +
        "=" +
        encodeURIComponent("10"); /**/
      queryParams +=
        "&" +
        encodeURIComponent("startCreateDt") +
        "=" +
        encodeURIComponent("20200310"); /**/
      queryParams +=
        "&" +
        encodeURIComponent("endCreateDt") +
        "=" +
        encodeURIComponent("20200315"); /**/
      const data = await axios.get(url + queryParams);
      console.log(data);
    },
  },
  mounted() {
    this.getCovidInfo();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.wrap {
  width: 552px;
  font-family: "GmarketSansMedium";
}

.date {
  font-size: 18px;
  text-align: right;
}

.icon {
  width: 20px;
  vertical-align: middle;
  margin-left: 5px;
  cursor: pointer;
}

.item-wrap {
  margin-top: 10px;
}

.item {
  box-sizing: border-box;
  width: 100%;
  font-size: 40px;
  font-weight: bold;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 30px;
  margin-bottom: 10px;
}

.item:nth-of-type(4n-3) {
  background: #f48032;
}
.item:nth-of-type(4n-2) {
  background: #ff7475;
}
.item:nth-of-type(4n-1) {
  background: #279d87;
}
.item:nth-of-type(4n) {
  background: #508ca9;
}
</style>
