<template>
  <main>
    <div
      class="alert alert-danger text-center"
      role="alert"
      v-if="isResponse === false"
    >
      City not found. Please try again.
    </div>
    <div class="search-box">
      <input
        type="text"
        class="search-bar"
        placeholder="Search"
        v-model="query"
        @keypress="fetchWeatherData"
      />
    </div>

    <a href="#cites-section" class="go-dwon">
      <i class="fas fa-angle-double-down fa-2x"></i>
    </a>

    <div class="container" v-for="city in citiesList" :key="city.id">
      <div class="card" style="width: 18rem">
        <h2 class="text-center card-header">Today</h2>
        <img
          class="card-img-top"
          v-bind:src="
            'https://openweathermap.org/img/wn/' + city.icon1 + '@4x.png'
          "
          alt="Card image cap"
        />
        <div class="card-body">
          <h4 class="card-title">{{ city.cityName }}, {{ city.country }}</h4>
          <p class="card-text">Tempareture: {{ city.tempToday }}</p>
          <p class="card-text">State: {{ city.mainToday }}</p>
        </div>
        <button
          @click="removeCity(city)"
          type="button"
          class="btn btn-danger remove-button"
          v-if="citiesList.length > 1"
        >
          X
        </button>
      </div>
      <div class="card" style="width: 18rem">
        <h2 class="text-center card-header">Tommorrow</h2>
        <img
          class="card-img-top"
          v-bind:src="
            'https://openweathermap.org/img/wn/' + city.icon2 + '@4x.png'
          "
          alt="Card image cap"
        />
        <div class="card-body">
          <h4 class="card-title">{{ city.cityName }}, {{ city.country }}</h4>
          <p class="card-text">Tempareture: {{ city.tempTommorrow }}</p>
          <p class="card-text">State: {{ city.mainTommorrow }}</p>
        </div>
        <button
          v-if="citiesList.length > 1"
          type="button"
          class="btn btn-danger remove-button"
          @click="removeCity(city)"
        >
          X
        </button>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "d9fea2949fefcb44526952250f6fa858",
      url_base: "https://api.openweathermap.org/data/2.5/forecast",
      query: "",
      citiesList: [],
      defaultID: 1,
      isResponse: true,
    };
  },
  created() {
    fetch(`${this.url_base}?q=Amsterdam&appid=${this.api_key}`)
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        this.citiesList.push({
          id: 0,
          cityName: "Amsterdam",
          country: "NL",
          tempToday: data.list[0].main.temp,
          tempTommorrow: data.list[1].main.temp,
          mainToday: data.list[0].weather[0].main,
          mainTommorrow: data.list[1].weather[0].main,
          icon1: data.list[0].weather[0].icon,
          icon2: data.list[1].weather[0].icon,
        });
      });
  },

  methods: {
    fetchWeatherData(event) {
      if (event.key == "Enter") {
        fetch(`${this.url_base}?q=${this.query}&appid=${this.api_key}`)
          .then((res) => {
            return res.json();
          })
          .then((res) => {
            if (res.cod == "404") {
              this.isResponse = false;
            } else {
              this.isResponse = true;
            }
            this.addCity(res);
          });
      }
    },

    addCity(city) {
      this.citiesList.push({
        id: this.defaultID,
        cityName: city.city.name,
        country: city.city.country,
        tempToday: city.list[0].main.temp,
        tempTommorrow: city.list[1].main.temp,
        mainToday: city.list[0].weather[0].main,
        mainTommorrow: city.list[1].weather[0].main,
        icon1: city.list[0].weather[0].icon,
        icon2: city.list[1].weather[0].icon,
      });
      this.defaultID++;
    },

    removeCity(city) {
      if (this.citiesList.length > 1) {
        this.citiesList = this.citiesList.filter((x) => {
          return x.id != city.id;
        });
      }
    },
  },
};
</script>

<style>
main .container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly;
}

main .card-header {
  background-color: #343a40;
  color: white;
  padding: 20px;
}
main .search-bar {
  display: block;
  padding: 20px 30px;
  border-radius: 100px;
  border: 3px solid #343a40;
  font-size: 18px;
  width: 30%;
  height: 30px;
  margin: auto;
}

.card {
  margin-bottom: 20px;
  box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px,
    rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px,
    rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;
}

.card-body {
  background-color: #494850;
  color: white;
}

.card-text {
  font-size: 20px;
}

.remove-button {
  position: absolute;
  border-radius: 100%;
  font-size: 14px;
  top: 10px;
  left: 5px;
}
</style>
