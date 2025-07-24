<script>
    import axios from 'axios';
    import {
        MapPinIcon,
        CloudIcon,
        Bars3Icon,
        AdjustmentsVerticalIcon,
        BeakerIcon,
    } from '@heroicons/vue/24/solid';
    const API_KEY = import.meta.env.VITE_OPENWEATHER_API_KEY;

    export default {
        components: {
            MapPinIcon,
            CloudIcon,
            Bars3Icon,
            AdjustmentsVerticalIcon,
            BeakerIcon,
        },
        data() {
            return {
                city: '',
                error: '',
                info: null,
                loading: false,
            };
        },
        computed: {
            cityName() {
                return this.city ? `"${this.city}"` : 'вашем городе';
            },
            canSearch() {
                return this.city.trim().length > 2 && !this.loading;
            },
            weatherIconUrl() {
                if (this.info && this.info.weather && this.info.weather[0]) {
                    return `https://openweathermap.org/img/wn/${this.info.weather[0].icon}@4x.png`;
                }
                return '';
            },
        },
        watch: {
            city() {
                this.error = '';
                this.info = null;
            },
        },
        mounted() {
            this.$nextTick(() => {
                this.$el.querySelector('input')?.focus();
            });
        },
        methods: {
            async getWeather() {
                if (!this.canSearch) return;
                this.error = '';
                this.info = null;
                this.loading = true;
                try {
                    const response = await axios.get(
                        `https://api.openweathermap.org/data/2.5/weather`,
                        {
                            params: {
                                q: this.city,
                                appid: API_KEY,
                                units: 'metric',
                                lang: 'ru',
                            },
                        }
                    );
                    this.info = response.data;
                } catch (err) {
                    if (err.response && err.response.status === 404) {
                        this.error = 'Город не найден. Проверьте название.';
                    } else if (err.response && err.response.status === 401) {
                        this.error = 'Ошибка авторизации API. Проверьте ключ.';
                    } else {
                        this.error =
                            'Ошибка загрузки данных. Попробуйте позже.';
                    }
                } finally {
                    this.loading = false;
                }
            },
        },
    };
</script>

<template>
    <div class="app-bg">
        <div class="weather-card">
            <h1 class="title">Погода</h1>
            <form
                class="search-form"
                @submit.prevent="getWeather">
                <div class="input-group">
                    <MapPinIcon
                        class="input-icon"
                        style="width: 22px; height: 22px; color: #8ab4f8" />
                    <input
                        type="text"
                        v-model="city"
                        placeholder="Введите город"
                        :disabled="loading"
                        maxlength="32"
                        autocomplete="off" />
                    <button
                        class="search-btn"
                        :disabled="!canSearch"
                        type="submit">
                        <span v-if="!loading">Найти</span>
                        <span
                            v-else
                            class="loader"></span>
                    </button>
                </div>
            </form>
            <transition name="fade">
                <div
                    v-if="error"
                    class="error-msg">
                    {{ error }}
                </div>
            </transition>
            <transition name="fade">
                <div
                    v-if="info && info.main"
                    class="weather-info">
                    <div class="weather-main">
                        <img
                            :src="weatherIconUrl"
                            :alt="info.weather[0].description"
                            class="weather-icon" />
                        <div class="temp-block">
                            <span class="temp"
                                >{{ Math.round(info.main.temp) }}°C</span
                            >
                            <span class="desc">{{
                                info.weather[0].description
                            }}</span>
                        </div>
                    </div>
                    <div class="weather-details-col">
                        <div class="detail detail-full">
                            <AdjustmentsVerticalIcon
                                style="
                                    width: 20px;
                                    height: 20px;
                                    color: #90caf9;
                                " />
                            <span
                                >Ощущается как:
                                <b
                                    >{{ Math.round(info.main.feels_like) }}°C</b
                                ></span
                            >
                        </div>
                        <div class="weather-details-row">
                            <div class="detail detail-half">
                                <BeakerIcon
                                    style="
                                        width: 20px;
                                        height: 20px;
                                        color: #64b5f6;
                                    " />
                                <span
                                    >Влажность:
                                    <b>{{ info.main.humidity }}%</b></span
                                >
                            </div>
                            <div class="detail detail-half">
                                <Bars3Icon
                                    style="
                                        width: 20px;
                                        height: 20px;
                                        color: #b3e5fc;
                                    " />
                                <span
                                    >Ветер:
                                    <b>{{ info.wind.speed }} м/с</b></span
                                >
                            </div>
                        </div>
                    </div>
                    <div class="city-name">
                        {{ info.name }}, {{ info.sys.country }}
                    </div>
                </div>
            </transition>
            <transition name="fade">
                <div
                    v-if="!info && !error && !loading"
                    class="placeholder">
                    <CloudIcon
                        style="width: 60px; height: 60px; color: #fff8" />
                    <p>Введите город и нажмите «Найти»</p>
                </div>
            </transition>
        </div>
    </div>
</template>

<style scoped>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

    .app-bg {
        min-height: 100vh;
        min-width: 100vw;
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        display: flex;
        align-items: center;
        justify-content: center;
        font-family: 'Inter', 'Segoe UI', Arial, sans-serif;
    }
    .weather-card {
        background: rgba(255, 255, 255, 0.15);
        box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
        backdrop-filter: blur(12px);
        -webkit-backdrop-filter: blur(12px);
        border-radius: 32px;
        border: 1px solid rgba(255, 255, 255, 0.18);
        padding: 48px 38px 38px 38px;
        min-width: 370px;
        max-width: 98vw;
        width: 420px;
        min-height: 560px;
        display: flex;
        flex-direction: column;
        align-items: center;
        transition: box-shadow 0.3s;
    }
    .title {
        font-size: 2.2rem;
        font-weight: 600;
        color: #fff;
        margin-bottom: 24px;
        letter-spacing: 1px;
        text-shadow: 0 2px 8px #0002;
    }
    .search-form {
        width: 100%;
        margin-bottom: 24px;
    }
    .input-group {
        display: flex;
        align-items: center;
        background: rgba(255, 255, 255, 0.25);
        border-radius: 14px;
        box-shadow: 0 4px 16px #0001;
        padding: 8px 12px 8px 16px;
        transition: box-shadow 0.2s;
        width: 100%;
        min-height: 48px;
        position: relative;
    }
    .input-icon {
        margin-right: 8px;
        flex-shrink: 0;
    }
    input[type='text'] {
        flex: 1;
        background: transparent;
        border: none;
        outline: none;
        color: #fff;
        font-size: 1.15rem;
        padding: 8px 6px 8px 0;
        font-family: inherit;
        letter-spacing: 0.5px;
        min-width: 0;
    }
    input[type='text']::placeholder {
        color: #e0e0e0cc;
        font-weight: 400;
    }
    .search-btn {
        background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
        color: #fff;
        border: none;
        border-radius: 10px 14px 14px 10px;
        padding: 8px 22px;
        font-size: 1rem;
        font-weight: 600;
        margin-left: 0;
        cursor: pointer;
        box-shadow: 0 2px 8px #0002;
        transition: background 0.2s, transform 0.1s;
        display: flex;
        align-items: center;
        min-width: 70px;
        justify-content: center;
        position: absolute;
        right: 8px;
        top: 50%;
        transform: translateY(-50%);
        height: 36px;
    }
    .search-btn:disabled {
        background: #bdbdbd;
        color: #fff;
        cursor: not-allowed;
        box-shadow: none;
    }
    .search-btn:not(:disabled):active {
        transform: translateY(-50%) scale(0.97);
    }
    .loader {
        border: 3px solid #fff3;
        border-top: 3px solid #fff;
        border-radius: 50%;
        width: 18px;
        height: 18px;
        animation: spin 0.8s linear infinite;
        display: inline-block;
    }
    @keyframes spin {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }
    .error-msg {
        color: #ff5252;
        background: rgba(255, 255, 255, 0.18);
        border-radius: 8px;
        padding: 10px 16px;
        margin-top: 10px;
        font-size: 1rem;
        font-weight: 500;
        text-align: center;
        box-shadow: 0 2px 8px #0001;
    }
    .weather-info {
        margin-top: 24px;
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .weather-main {
        display: flex;
        align-items: center;
        gap: 22px;
    }
    .weather-icon {
        width: 96px;
        height: 96px;
        filter: drop-shadow(0 2px 8px #0002);
    }
    .temp-block {
        display: flex;
        flex-direction: column;
        align-items: flex-start;
    }
    .temp {
        font-size: 2.8rem;
        font-weight: 600;
        color: #fff;
        text-shadow: 0 2px 8px #0002;
    }
    .desc {
        font-size: 1.1rem;
        color: #e0e0e0;
        margin-top: 2px;
        text-transform: capitalize;
    }
    .weather-details {
        display: flex;
        flex-wrap: wrap;
        gap: 14px 22px;
        margin-top: 22px;
        justify-content: center;
    }
    .detail {
        display: flex;
        align-items: center;
        gap: 7px;
        background: rgba(255, 255, 255, 0.13);
        border-radius: 10px;
        padding: 8px 16px;
        font-size: 1rem;
        color: #fff;
        box-shadow: 0 1px 4px #0001;
    }
    .city-name {
        margin-top: 22px;
        font-size: 1.1rem;
        color: #fff;
        font-weight: 500;
        letter-spacing: 0.5px;
        text-shadow: 0 2px 8px #0002;
    }
    .placeholder {
        margin-top: 36px;
        color: #fff8;
        font-size: 1.1rem;
        text-align: center;
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 10px;
    }
    .fade-enter-active,
    .fade-leave-active {
        transition: opacity 0.3s;
    }
    .fade-enter-from,
    .fade-leave-to {
        opacity: 0;
    }
    .weather-details-col {
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 18px;
        margin-top: 24px;
    }
    .weather-details-row {
        width: 100%;
        display: flex;
        flex-direction: row;
        gap: 16px;
        justify-content: center;
    }
    .detail-full {
        width: 100%;
        justify-content: center;
    }
    .detail-half {
        width: 48%;
        min-width: 120px;
        justify-content: center;
    }
    .detail {
        display: flex;
        align-items: center;
        gap: 7px;
        background: rgba(255, 255, 255, 0.18);
        border-radius: 12px;
        padding: 10px 0;
        font-size: 1.05rem;
        color: #fff;
        box-shadow: 0 1px 4px #0001;
        margin: 0;
        text-align: center;
        font-weight: 500;
    }
    @media (max-width: 600px) {
        .weather-card {
            padding: 18px 2vw 18px 2vw;
            min-width: unset;
            width: 98vw;
            min-height: 0;
        }
        .weather-main {
            flex-direction: column;
            gap: 10px;
        }
        .weather-icon {
            width: 70px;
            height: 70px;
        }
        .temp {
            font-size: 2.1rem;
        }
        .input-group {
            padding: 8px 8px 8px 12px;
        }
        .search-btn {
            padding: 8px 14px;
            min-width: 54px;
            height: 32px;
            font-size: 0.95rem;
        }
    }
</style>

