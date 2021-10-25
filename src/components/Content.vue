<template>
  <div class="wrap">
    <div class="item-wrap">
      <div class="item">
        <div class="label">확진자 수</div>
        <div class="data">{{ decideCnt }}명 ({{ compDecideCnt }})</div>
      </div>
      <div class="item">
        <div class="label">사망자 수</div>
        <div class="data">{{ deathCnt }}명 ({{ compDeathCnt }})</div>
      </div>
      <div class="item">
        <div class="label">치료중 환자 수</div>
        <div class="data">{{ careCnt }}명</div>
      </div>
      <div class="item">
        <div class="label">누적 검사 수</div>
        <div class="data">{{ accExamCnt }}건</div>
      </div>

      <div class="item">
        <div class="label">누적 확진률</div>
        <div class="data">{{ accDefRate }}%</div>
      </div>
    </div>
    <div class="date">
      <span>{{ date }} 00시 기준</span>
      <img
        src="@/assets/refresh.png"
        alt="refresh"
        class="icon"
        @click="getCovidInfo"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import dayjs from "dayjs";

export default {
  name: "Content",
  data() {
    return {
      accDefRate: 0,
      careCnt: 0,
      deathCnt: 0,
      decideCnt: 0,
      date: "",
      compDeathCnt: 0,
      compDecideCnt: 0,
      accExamCnt: 0,
    };
  },
  methods: {
    async getCovidInfo() {
      const today = dayjs().format("YYYYMMDD");
      const yesterday = dayjs(today)
        .subtract(1, "day")
        .format("YYYYMMDD");

      let url = "/openapi/service/rest/Covid19/getCovid19InfStateJson"; /*URL*/
      let queryParams =
        "?" +
        encodeURIComponent("serviceKey") +
        "=" +
        "GGw58UOnRwx4EftqiTrQ7LFYo6ATZicDZ5H4IG%2F16UAy0m9ExsZys530QgmoUfvQkJQAO4WV71IHRnJpEndp1w%3D%3D"; /*Service Key*/
      queryParams +=
        "&" + encodeURIComponent("pageNo") + "=" + encodeURIComponent("1"); /**/
      queryParams +=
        "&" + encodeURIComponent("numOfRows") + "=" + encodeURIComponent("10");
      queryParams +=
        "&" +
        encodeURIComponent("startCreateDt") +
        "=" +
        encodeURIComponent(yesterday);
      queryParams +=
        "&" +
        encodeURIComponent("endCreateDt") +
        "=" +
        encodeURIComponent(today);

      const res = await axios.get(url + queryParams);
      const data = res.data.response.body.items.item;
      const todayData = data.find((item) => item.stateDt === parseInt(today));
      const yesterdayData = data.find(
        (item) => item.stateDt === parseInt(yesterday)
      );

      this.accDefRate = Number(todayData.accDefRate).toFixed(2);
      this.careCnt = todayData.careCnt.toLocaleString();
      this.deathCnt = todayData.deathCnt.toLocaleString();
      this.decideCnt = todayData.decideCnt.toLocaleString();
      this.accExamCnt = todayData.accExamCnt.toLocaleString();
      this.date = dayjs(todayData.stateDt.toString()).format(
        "YYYY년 MM월 DD일"
      );
      this.compDeathCnt =
        todayData.deathCnt - yesterdayData.deathCnt > 0
          ? `+${(todayData.deathCnt - yesterdayData.deathCnt).toLocaleString()}`
          : (todayData.deathCnt - yesterdayData.deathCnt).toLocaleString();
      this.compDecideCnt =
        todayData.decideCnt - yesterdayData.decideCnt > 0
          ? `+${(
              todayData.decideCnt - yesterdayData.decideCnt
            ).toLocaleString()}`
          : (todayData.decideCnt - yesterdayData.decideCnt).toLocaleString();
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

.icon:hover {
  animation: rotate 1s;
}

.item-wrap {
  margin-top: 10px;
}

.item {
  box-sizing: border-box;
  width: 100%;
  font-size: 35px;
  font-weight: bold;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 20px;
  margin-bottom: 10px;
}

.item:nth-of-type(5n-4) {
  background: #f48032;
}
.item:nth-of-type(5n-3) {
  background: #ff7475;
}
.item:nth-of-type(5n-2) {
  background: #279d87;
}
.item:nth-of-type(5n-1) {
  background: #9d8aca;
}
.item:nth-of-type(5n) {
  background: #508ca9;
}

@keyframes rotate {
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
}
</style>
