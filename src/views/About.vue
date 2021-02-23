<template>
    <div id="about" class="content">
        <main class="search-mode">
            <div class="search-box">
                <input type="text"
                       class="search-bar"
                       placeholder="도시명을 입력해주세요."
                       v-model="query"
                       @keypress="fetchWeather"
                />
                <input type="button"
                       value="검색"
                       @click="clickSearch"
                />
            </div>
            <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
                <div class="location-box">
                    <p class="location">{{ weather.name }}, {{weather.sys.country}}</p>
                    <p class="date">{{ dateBuilder() }}</p>
                </div>
                <div class="weather-box">
                    <p class="temp">{{ weather.main.temp | round }}℃</p>
                    <p class="weather">{{ weather.weather[0].main }}</p>
                </div>
            </div>
        </main>
    </div>
</template>
<script>
export default {
    name: "about",
    data: function () {
        return {
            keyValue: "5d10b56c72a223d178342aaef3d01c42",
            baseUrl: "https://api.openweathermap.org/data/2.5/",
            query: "",
            weather: {}
        };
    },
    methods: {
        fetchWeather: function (e){
            if(e.key == "Enter"){
                let fetchUrl = `${this.baseUrl}weather?q=${this.query}&units=metric&APPID=${this.keyValue}`;
                fetch(fetchUrl)
                .then((res) => {
                    return res.json();
                })
                .then((results) => {
                    return this.setResult(results);
                })
            }
        },
        clickSearch: function (){
            let fetchUrl = `${this.baseUrl}weather?q=${this.query}&units=metric&APPID=${this.keyValue}`;
            fetch(fetchUrl)
                    .then((res) => {
                        return res.json();
                    })
                    .then((results) => {
                        return this.setResult(results);
                    })
        },
        setResult: function (results){
            this.weather = results;
        },
        dateBuilder: function (){
            let d = new Date();
            let months = [
                "1월",
                "2월",
                "3월",
                "4월",
                "5월",
                "6월",
                "7월",
                "8월",
                "9월",
                "10월",
                "11월",
                "12월"
            ];
            let days = [
                "월요일",
                "화요일",
                "수요일",
                "목요일",
                "금요일",
                "토요일",
                "일요일"
            ];
            let day = days[d.getDay()];
            let date = d.getDate();
            let month = months[d.getMonth()];
            let year = d.getFullYear();
            return `${year}년 ${month} ${date}일 ${day}`
        }
    },
    filters: {
        round: function (val){
            return Math.round(val * 10) / 10
        }
    }
}
</script>
<style lang="scss">
</style>
