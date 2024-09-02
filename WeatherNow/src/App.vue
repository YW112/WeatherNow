<style>

</style>

<template>
  <div class="weather-app" :class="weatherClass">
    <div class="wrapper">
      <div class="el weather-view">
        <input class="weather-view__entr" v-model="searchQuery" placeholder="Enter city" @keyup.enter="weatherSearch" type="text">
        <button class="weather-view__btn" @click="weatherSearch">Search</button>
      </div>
      <div class="block weather-load" v-if="loading">Loading...</div>
      <div class="weather-details" v-show="!error && location && temperature !== 0 && description">
      <div class="block" v-if="error">error</div>
        <div class="weather-details__txt">
          <p class="block">{{ location }}</p>
          <p class="block">{{ temperature }}°C</p>
          <p class="block">{{ description }}</p>
        </div>
      </div>
    </div>
    <div class="weather-bg">
      <div>
        <img class="weather-bg__img bg" src="./assets/bg-default.jpg" alt="Earth in space">
        <img class="weather-bg__img cloudy" src="./assets/cloudy.jpg" alt="Cloudy weather outside">
        <img class="weather-bg__img rainy" src="./assets/rainy.jpg" alt="Rainy weather outside">
        <img class="weather-bg__img sunny" src="./assets/sunny.jpg" alt="Sunny weather outside">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      location: '',
      temperature: 0,
      description: '',
      loading: false,
      error: false,
      searchQuery: '',
    };
  },
  computed: {
    weatherClass() {
      if (this.description.includes('Sunny')) {
        return 'sunny';
      } else if (this.description.includes('Overcast') || this.description.includes('rain') || this.description.includes('drizzle')) {
        return 'rainy';
      }
      else if (this.description.includes('Partly cloudy')) {
        return 'cloudy';
      } else {
        return 'bg';
      }
    }
  },
  methods: {
    weatherSearch() {
      this.loading =  true;
      this.error = false;
      fetch(`https://api.weatherapi.com/v1/current.json?key=f40e4a42b37740279ce175530242508&q=${this.searchQuery}`)
      .then(response => response.json())
      .then(data => {
        this.loading = false;
        this.location = data.location.name;
        this.temperature = data.current.temp_c;
        this.description = data.current.condition.text;
        this.resetSearchQuery();
      })
      .catch(error => {
        this.loading = false;
        this.error = true;
        console.error(error);
      })
    },
    resetSearchQuery() {
      
    }
  }
}
</script>