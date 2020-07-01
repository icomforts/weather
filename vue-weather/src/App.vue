<template>
  <div id="app" :class="pop > 60 ? 'rain' : pop < 30 ? 'sun' : ''">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="地點..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap">
        <div class="location-box">
          <div class="location">{{ weather.locationName }}</div>
          <div class="pop">降雨機率:&nbsp;{{ pop }}%</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ mint }}-{{ maxt }}°C</div>
          <div class="weather">{{ parameter }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "CWB-C6596524-57DD-4D5B-BA38-EB4A2244DB3B",
      url_base: "https://opendata.cwb.gov.tw/api/",
      query: "",
      weather: "",
      parameter: "",
      mint: "",
      maxt: "",
      pop: "",
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}v1/rest/datastore/F-C0032-001?Authorization=${this.api_key}&locationName=${this.query}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults)
          .catch(() => {
            alert("可能輸入錯誤囉! ex:臺北市");
          });
      }
    },
    setResults(results) {
      this.weather = results.records.location[0];
      this.parameter = this.weather.weatherElement[0].time[0].parameter.parameterName;
      this.mint =
        results.records.location[0].weatherElement[2].time[1].parameter.parameterName;
      this.maxt =
        results.records.location[0].weatherElement[4].time[1].parameter.parameterName;
      this.pop =
        results.records.location[0].weatherElement[1].time[2].parameter.parameterName;
    },
  },
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
}
#app {
  background: url(https://images.pexels.com/photos/1431822/pexels-photo-1431822.jpeg?auto=compress&cs=tinysrgb&h=750&w=1260);
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.rain {
  background: url(https://images.pexels.com/photos/459451/pexels-photo-459451.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260);
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.sun {
  background: url(https://images.pexels.com/photos/1234064/pexels-photo-1234064.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260);
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: azure;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);

  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .pop {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
