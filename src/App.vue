<script setup>
import axios from "axios";
import { ref } from "vue";

const city = ref("");
const info = ref(null);
const maxTemp = ref(null);
const minTemp = ref(null);
const windSpeed = ref(null);
const feelTemp = ref(null);
const humidity = ref(null);
const search = () => {
  axios
    .get(
      `https://api.openweathermap.org/data/2.5/weather?q=${city.value}&units=metric&appid=1eb9a1c2b6a76d62d0d9c367f40ffbdd`
    )
    .then((res) => {
      info.value = res.data.main.temp.toFixed(1) + "°C";
      maxTemp.value = res.data.main.temp_max + "°C";
      minTemp.value = res.data.main.temp_min + "°C";
      windSpeed.value = res.data.wind.speed + " m.s";
      feelTemp.value = res.data.main.feels_like + "°C";
      humidity.value = res.data.main.humidity + "%";
    })
    .catch(function (error) {
      alert(`Палундра, Ты ввел херню: ${error.response.status}.`);
    });
};

const clear = () => {
  city.value = "";
  info.value = null;
  maxTemp.value = null;
  minTemp.value = null;
  windSpeed.value = null;
  feelTemp.value = null;
  humidity.value = null;
};
</script>

<template>
  <div class="container">
    <h1>Приложение погоды</h1>

    <p>Узнать погоду в вашем городе {{ info }}</p>

    <input type="text" placeholder="Введите город" v-model="city" />

    <div class="wrapper__btn">
      <button @click="search()">
        Проверить погоду
        <span v-show="city.length !== 0">в {{ city }}</span>
      </button>

      <button @click="clear()">Очистить поле</button>
    </div>

    <div class="wrapper__temp">
      <table v-show="info !== null">
        <tr>
          <th>Параметры</th>
          <th>Значения</th>
        </tr>
        <tr>
          <td>Температура:</td>
          <td>{{ info }}</td>
        </tr>
        <tr>
          <td>Максимальная температура:</td>
          <td>{{ maxTemp }}</td>
        </tr>
        <tr>
          <td>Минимальная температура:</td>
          <td>{{ minTemp }}</td>
        </tr>
        <tr>
          <td>Ощущается как:</td>
          <td>{{ feelTemp }}</td>
        </tr>
        <tr>
          <td>Влажность:</td>
          <td>{{ humidity }}</td>
        </tr>
        <tr>
          <td>Скорость ветра:</td>
          <td>{{ windSpeed }}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 50vw;
  height: 70vh;
  border-radius: 50px;
  background: #5cdb95;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 15px;
  padding: 25px;
}

.container h1 {
  font-size: 36px;
}
.container p {
  font-size: 20px;
}
.container input {
  display: block;
  width: 100%;
  height: calc(2.25rem + 2px);
  padding: 0.375rem 0.75rem;
  font-family: inherit;
  font-size: 1rem;
  font-weight: 400;
  line-height: 1.5;
  color: #212529;
  background-color: #edf5e1;
  background-clip: padding-box;
  border: 1px solid #bdbdbd;
  border-radius: 0.25rem;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}
.container button {
  background-color: #8ee4af;
  width: 260px;
  padding: 15px 0;
  border-radius: 30px;
  transition: all 0.5s ease-out;
}
.container button:hover {
  background-color: #379683;
  color: #edf5e1;
}

.wrapper__btn {
  display: flex;
  gap: 20px;
}

.wrapper__temp {
  width: 40vw;
  height: 40vh;
  background: #edf5e1;
  display: flex;
  justify-content: center;
  align-items: center;
}

.wrapper__temp table {
  width: inherit;
  height: inherit;
  border: 3px solid #05386b;
  padding: 0;
  margin: 0;
  font-size: 20px;
}
.wrapper__temp th,
td {
  border: 1px solid #05386b;
  padding: 10px;
}
</style>
