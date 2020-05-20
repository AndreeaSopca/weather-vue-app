<template>
    <div id="app" :class="warm ? 'warm': ''">

        <main>
            <div class="search-box">
                <input type="text"
                       class="search-bar"
                       placeholder="Search ..."
                       v-model="query"
                       @keypress="fetchWeather"
                />
            </div>
            <div class="weather-wrap" v-if="ready">
                <div class="location-box">
                    <div class="location">{{weather.name}} {{weather.sys.country}}</div>
                    <div class="date">{{dateBuilder()}}</div>
                </div>

                <div class="weather-box">
                    <div class="temp">{{Math.round(weather.main.temp) }}</div>
                    <div class="weather">{{weather.weather[0].main}}</div>
                </div>
            </div>
        </main>


    </div>
</template>

<script>

    export default {
        name: 'App',
        data() {
            return {
                api_key: 'b1a3e5c492d0266bc25172faa146fefb',
                url_base: 'https://api.openweathermap.org/data/2.5/',
                query: '',
                weather: {}
            }
        },

        computed: {
            ready: function () {
                return typeof this.weather.main !== 'undefined';
            },
            warm: function () {
                return ((typeof this.weather.main !== 'undefined') && (this.weather.main.temp));
            }
        },

        methods: {
            fetchWeather(e) {
                if (e.key === "Enter") {
                    fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
                        .then(res => {
                            return res.json()
                        })
                        .then(this.setResults);
                }
            },

            setResults(results) {
                this.weather = results
            },

            dateBuilder() {
                let d = new Date();
                console.log(d.getDate());
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

<style>
    * {
        margin: 0 auto;
        padding: 0;
        box-sizing: border-box;
        max-width: 500px;
    }

    body {
        font-family: 'montserrat', sans-serif;
    }

    #app {
        background-image: url('./assets/cold-bg.jpg');
        background-size: cover;
        background-position: bottom;
        transition: 0.4s;
    }

    #app.warm {
        background-image: url('./assets/warm-bg.jpg');
    }

    main {
        min-height: 100vh;
        padding: 25px;

        background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
    }

    .search-box {
        width: 100%;
        margin-bottom: 30px;
    }

    .search-box .search-bar {
        width: 100%;
        display: block;
        padding: 15px;

        color: slategray;
        font-size: 20px;

        appearance: none;
        border: none;
        outline: none;
        background: rgba(255, 255, 255, 0.5) none;
        box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
        border-radius: 0 16px 0 16px;
        transition: 0.4s;
    }

    .search-box .search-bar:focus {
        box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
        background: rgba(255, 255, 255, 0.75) none;
        border-radius: 16px 0 16px 0;
    }

    .location-box .location {
        color: #FFFFFF;
        font-size: 32px;
        font-weight: 500;
        text-align: center;
        text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
    }

    .location-box .date {
        color: #FFFFFF;
        font-size: 20px;
        font-weight: 300;
        text-align: center;
        font-style: italic;
    }

    .weather-box {
        text-align: center;
    }

    .weather-box .temp {
        display: inline-block;
        padding: 10px 25px;
        color: #FFFFFF;
        font-size: 100px;
        font-weight: 900;

        text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
        background-color: rgba(255, 255, 255, 0.25);
        border-radius: 16px;
        margin: 30px 0;

        box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }

    .weather-box .weather {
        font-size: 48px;
        color: #FFFFFF;
        font-weight: 700;
        font-style: italic;
        text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }


</style>
