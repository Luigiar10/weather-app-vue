<template>
   <div class="app">
      <main
      class="container"
      :class="
         typeof weather.main != 'undefined' && weather.main.temp > 16
            ? 'warm'
            : ''
      "
   >
      <div class="search-box">
         <input
            type="text"
            placeholder="Buscar..."
            v-model="cityName"
            @keyup.enter="callApiWeather"
         />
      </div>
      <div class="information-box" v-if="typeof weather.main != 'undefined'">
         <div class="country">
            <p>{{ weather.name.toUpperCase() }}, {{ weather.sys.country }}</p>
         </div>
         <div class="date">
            <p>{{ dateBuilder() }}</p>
         </div>
         <div class="temperature">
            <p>{{ Math.round(weather.main.temp) }}°C</p>
         </div>
         <div class="more-info">
            <p>{{ weather.weather[0].description }}</p>
         </div>
      </div>
      <div v-else class="loading">
         <p>Por favor, introduce una ciudad</p>
      </div>
      <div class="person">
         <p><a target="_BLANK" href="https://github.com/luigiar10">Luigi Arrieta</a></p>
      </div>
   </main>
   </div>
</template>

<script>
export default {
   name: "App",

   data() {
      return {
         cityName: "",
         api_key: "9350bc6a2103b851e123f31b31a64114",
         url_base: "https://api.openweathermap.org/data/2.5",
         units: "Metric",
         weather: {},
      };
   },

   methods: {
      callApiWeather() {
         try {
            fetch(
               `${this.url_base}/weather?q=${this.cityName}&appid=${this.api_key}&units=${this.units}&lang=es`
            )
               .then((response) => response.json())
               .then(this.resultCallApi);
         } catch (error) {
            console.log("Error: " + error);
         }
      },

      resultCallApi(results) {
         this.weather = results;
      },

      dateBuilder() {
         let d = new Date();
         let months = [
            "Enero",
            "Febrero",
            "Marzo",
            "Abril",
            "Mayo",
            "Junio",
            "Julio",
            "Agosto",
            "Septiembre",
            "Octubre",
            "Noviembre",
            "Diciembre",
         ];
         let days = [
            "Domingo",
            "Lunes",
            "Martes",
            "Miercoles",
            "Jueve",
            "Viernes",
            "Sabado",
         ];
         let day = days[d.getDay()];
         let date = d.getDate();
         let month = months[d.getMonth()];
         let year = d.getFullYear();
         return `${day} ${date} ${month} ${year}`;
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

body,
html {
   font-family: "Monserrat", sans-serif;
   font-size: 62.5%;
}

.app{
   background-image: url("assets/cold-bg.jpg");
   background-size: contain;
   background-position: center;
   transition: 0.5s;
}

.container {
   min-height: 100vh;
   min-width: 100vw;
   width: 100%;
   height: 100%;
   display: flex;
   flex-direction: column;
   align-items: center;
   background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.warm {
   background-image: url("assets/warm-bg.jpg");
   background-size: cover;
}

.search-box {
   width: 40vw;
   margin: 4rem 0 8rem 0;
   padding: 2rem;
   background-color: rgba(0, 0, 0, 0.25);
   border-radius: 0 5rem 0 5rem;
}

@media (max-width: 1000px) {
   .search-box {
      width: 60vw;
   }
}

@media (max-width: 760px) {
   .search-box {
      width: 90vw;
   }
}

.search-box input {
   background-color: transparent;
   border: none;
   color: #fff;
   font-size: 2.4rem;
   outline: none;
   padding: 1rem;
   width: 100%;
}

.loading p {
   color: #fff;
   font-size: 4rem;
   padding: calc(100vh - 70vh) 0;
   text-align: center;
}

@media (max-width: 1000px) {
   .loading p {
      font-size: 3rem;
   }
}

.information-box {
   text-align: center;
}

.information-box .country p {
   color: #fff;
   font-size: 3rem;
   font-weight: 900;
   text-shadow: 0.3rem 0.3rem 0.3rem rgba(0, 0, 0, 0.5);
}

.information-box .date p {
   color: #fff;
   font-size: 3rem;
   font-weight: 300;
   font-style: italic;
}

.information-box .temperature {
   font-size: 10rem;
   color: #fff;
   padding: 2rem;
   border-radius: 2rem;
   margin: 4rem 0;
   background-color: rgba(0, 0, 0, 0.45);
   box-shadow: 0.3rem 0.3rem 0.3rem rgba(0, 0, 0, 0.75);
}

.information-box .more-info p {
   color: #fff;
   font-size: 4rem;
}

.person {
   position: absolute;
   bottom: 2rem;
}

.person p a {
   text-decoration: none;
   font-size: 1.5rem;
   color: #fff;
}
</style>
