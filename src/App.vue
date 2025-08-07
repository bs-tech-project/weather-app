<template>
  <div class="app">
    <h1>Weather App</h1>

    <div class="search-box">
      <input
        v-model="city"
        @keyup.enter="getWeather"
        placeholder="🔍 Enter city name"
        @input="error=''"
      />
      <button @click="getWeather">Get Weather</button>
    </div>

    <div v-if="loading" class="loader"></div>

    <p v-if="error" class="error-msg">{{ error }}</p>

    <transition name="fade">
      <WeatherCard v-if="weather" :weather="weather" />
    </transition>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import WeatherCard from './components/WeatherCard.vue'

const city = ref('')
const weather = ref(null)
const error = ref(null)
const loading = ref(false)

const API_KEY = '3c1ded2929424f6389392008250708' // Replace with your WeatherAPI key

async function getWeather() {
  if (!city.value.trim()) {
    error.value = "Please enter city name";
    return
  }
  weather.value = null
  error.value = null
  loading.value = true

  try {
    const res = await fetch(
      `https://api.weatherapi.com/v1/current.json?key=${API_KEY}&q=${city.value}`
    )

    if (!res.ok) throw new Error('City not found')

    const data = await res.json()
    weather.value = data
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
}
</script>

<style>
body {
  margin: 0;
  font-family: 'Segoe UI', sans-serif;
  background: linear-gradient(to right, #83a4d4, #b6fbff);
  color: #333;
}

.app {
  max-width: 500px;
  margin: auto;
  padding: 40px 20px;
  text-align: center;
}

h1 {
  font-size: 2.2rem;
  margin-bottom: 30px;
  color: #fff;
}

.search-box {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-bottom: 25px;
}

input {
  padding: 12px 16px;
  width: 70%;
  border: none;
  border-radius: 30px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
  font-size: 16px;
  outline: none;
}

button {
  padding: 12px 20px;
  border: none;
  border-radius: 30px;
  background-color: #4fc3f7;
  color: white;
  font-weight: bold;
  cursor: pointer;
  transition: background 0.3s ease;
}

button:hover {
  background-color: #0288d1;
}

.loader {
  border: 6px solid #f3f3f3;
  border-top: 6px solid #2196f3;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  animation: spin 1s linear infinite;
  margin: 20px auto;
}

.error {
  color: red;
  font-weight: 600;
  margin-top: 10px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* Smooth fade for weather card */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.error-msg {
  color: #e53935;
  font-size: 14px;
  margin-top: 6px;
  margin-bottom: 10px;
  text-align: center;
}

/* Mobile responsiveness */
@media (max-width: 500px) {
  .search-box {
    flex-direction: column;
  }

  input {
    width: 90%;
  }

  button {
    width: 100%;
  }
}
</style>
