<template>
    <div id="weather-app"  :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
        <main>
            <div class="search-box">
                <input 
                    type="text"
                    class="search-bar"
                    @keyup="fetchWeather"
                    v-model="query"
                    placeholder="Search...">
            </div>
            <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
                <div class="location-box">
                    <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
                    <div class="date">{{ dateBuilder() }}</div>
                </div>

                <div class="weather-box">
                    <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
                    <div class="weather">{{ weather.weather[0].main }}</div>
                </div>
            </div>
        </main>
    </div>
</template>
<script>
export default {
    data: function () {
        return {
            url_base: 'https://community-open-weather-map.p.rapidapi.com/weather',
            query: '',
            weather: {}
        }
    },
    methods: {
        fetchWeather: function (e) {
            if(e.key == "Enter") {
                fetch(`${this.url_base}?q=${this.query}`, {
                    "method": "GET",
                    "headers": {
                        "x-rapidapi-host": "community-open-weather-map.p.rapidapi.com",
                        "x-rapidapi-key": "7e8254b9c5msh4b4ac823d5ae6f3p12381ejsne225a55fa8a4"
                    }
                })
                .then(response => {
                    return response.json();
                }).then (this.setResults)
                .catch(err => {
                    console.log(err);
                });
            }
        },
        setResults: function (result) {
            this.weather = result;
            console.log(this.weather);   
        },
        dateBuilder () {
            let d = new Date();
            let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
            let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
            let day = days[d.getDay()];
            let date = d.getDate();
            let month = months[d.getMonth()];
            let year = d.getFullYear();
            return `${day} ${date} ${month} ${year}`;
        }
    }
}
</script>