<template>
  <div class="container">
    <h1>Daily Weather</h1>
    <WeatherTable />
  </div>
</template>

<script>
import WeatherTable from './components/WeatherTable'

export default {
	components: { WeatherTable },
  
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Comfortaa:wght@300&family=IBM+Plex+Sans:wght@200&display=swap');
body {
  background: rgb(26,173,203);
  background: linear-gradient(90deg, rgba(26,173,203,1) 0%, rgba(21,198,151,1) 50%, rgba(205,152,16,1) 100%);
  margin: 0;
  padding: 0;
  font-family: 'Comfortaa', cursive;
  font-size: 30px;
  color: bisque;
  text-align: center;
}
h1 {
  margin: 30px 0 0;
  font-weight: 200;
}
</style>
