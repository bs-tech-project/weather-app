<template>
  <div class="card" v-if="weather">
    <h2>{{ weather.location.name }}, {{ weather.location.country }}</h2>
    <img :src="weather.current.condition.icon" :alt="weather.current.condition.text" />
    <p>{{ weather.current.condition.text }}</p>
    <p>🌡 Temp: {{ weather.current.temp_c }}°C</p>
    <p>💨 Wind: {{ weather.current.wind_kph }} kph</p>
    <p>🌅 Feels Like: {{ weather.current.feelslike_c }}°C</p>
  </div>
</template>

<script>
export default {
  props: ['weather']
}
</script>

<style scoped>
.card {
  background: white;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  animation: fadeIn 0.4s ease-in;
}
img {
  width: 80px;
  height: 80px;
}
@keyframes fadeIn {
  from { opacity: 0; transform: scale(0.95); }
  to { opacity: 1; transform: scale(1); }
}
</style>
