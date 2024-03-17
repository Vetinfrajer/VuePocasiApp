<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col">
        <div class="card">
          <div class="card-body">
            <h1 class="card-title">Aktuální místo</h1>
            <p class="card-text">Místo: {{ location }}</p>
            <p class="card-text">Teplota: {{ temperature }} °C</p>
            <p class="card-text">Pocitová teplota: {{ feelsLike }} °C</p>
            <p class="card-text">Popis: {{ description }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
  
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        location: '',
        temperature: '',
        feelsLike: '',
        description: ''
      };
    },
    mounted() {
      this.getLocation();
    },
    methods: {
      async getLocation() {
        // Získání polohy uživatele
        if (navigator.geolocation) {
          navigator.geolocation.getCurrentPosition(async (position) => {
            const { latitude, longitude } = position.coords;
            this.getWeather(latitude, longitude);
          });
        } else {
          alert('Geolocation není podporována vaším prohlížečem.');
        }
      },
      async getWeather(latitude, longitude) {
        // Získání aktuálního počasí
        const apiKey = '36baab55ba4038ec60931c69e42dfe77';
        const apiUrl = `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${apiKey}&units=metric`;
        
        try {
          const response = await axios.get(apiUrl);
          const data = response.data;
          this.location = data.name;
          this.temperature = data.main.temp;
          this.feelsLike = data.main.feels_like;
          this.description = data.weather[0].description;
        } catch (error) {
          console.error('Chyba při získávání počasí:', error);
        }
      }
    }
  };
  </script>
  
  <style>
  </style>
  