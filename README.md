# Vue 3 Weather App

Современное погодное приложение на Vue 3 + Vite с красивым UI и использованием OpenWeatherMap API.

![Vue 3 Weather App](https://img.shields.io/badge/Vue-3.4+-green.svg)
![Vite](https://img.shields.io/badge/Vite-5.0+-blue.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

## ✨ Особенности

-   🌤️ **Поиск погоды** по названию города (русский/английский)
-   📱 **Адаптивный дизайн** с современным glassmorphism эффектом
-   🎨 **Красивый UI** с плавными анимациями и переходами
-   ⚡ **Быстрая работа** на Vue 3 + Vite
-   🔒 **Безопасность** - API ключ в переменных окружения
-   🛡️ **Обработка ошибок** с понятными сообщениями
-   📊 **Детальная информация**: температура, влажность, ветер, "ощущается как"

## 🚀 Быстрый старт

### Предварительные требования

-   Node.js 18+
-   npm или yarn

### Установка

1. **Клонируйте репозиторий:**

    ```bash
    git clone https://github.com/FrankFMY/vue3js-weather-app.git
    cd vue3js-weather-app
    ```

2. **Установите зависимости:**

    ```bash
    npm install
    ```

3. **Настройте API ключ:**

    Создайте файл `.env` в корне проекта:

    ```env
    VITE_OPENWEATHER_API_KEY=ваш_api_ключ_здесь
    ```

    > ⚠️ **Важно:** Получите бесплатный API ключ на [OpenWeatherMap](https://openweathermap.org/api)

4. **Запустите проект:**

    ```bash
    npm run dev
    ```

5. **Откройте браузер:**
    ```
    http://localhost:5173
    ```

## 📁 Структура проекта

```
vue3js-weather-app/
├── src/
│   ├── App.vue          # Главный компонент
│   ├── main.js          # Точка входа
│   └── assets/
│       └── main.css     # Глобальные стили
├── public/              # Статические файлы
├── .env                 # Переменные окружения
├── vite.config.js       # Конфигурация Vite
└── package.json         # Зависимости
```

## 🛠️ Команды

```bash
# Разработка
npm run dev

# Сборка для продакшена
npm run build

# Предварительный просмотр сборки
npm run preview

# Линтинг
npm run lint
```

## 🎨 Технологии

-   **Vue 3** - Прогрессивный JavaScript фреймворк
-   **Vite** - Быстрый инструмент сборки
-   **Axios** - HTTP клиент для API запросов
-   **Heroicons** - Красивые SVG иконки
-   **OpenWeatherMap API** - Данные о погоде

## 🔧 Конфигурация

### Переменные окружения

| Переменная                 | Описание                | Пример                             |
| -------------------------- | ----------------------- | ---------------------------------- |
| `VITE_OPENWEATHER_API_KEY` | API ключ OpenWeatherMap | `b7be2f6c76292a9201b44208a6c87570` |

### API Endpoints

Приложение использует следующие эндпоинты OpenWeatherMap:

-   `GET /data/2.5/weather` - Текущая погода по городу

## 🚀 Деплой

### Vercel

```bash
npm run build
vercel --prod
```

### Netlify

```bash
npm run build
# Загрузите папку dist на Netlify
```

### GitHub Pages

```bash
npm run build
# Настройте GitHub Actions для автоматического деплоя
```

## 🤝 Вклад в проект

1. Форкните репозиторий
2. Создайте ветку для новой функции (`git checkout -b feature/amazing-feature`)
3. Зафиксируйте изменения (`git commit -m 'Add amazing feature'`)
4. Отправьте в ветку (`git push origin feature/amazing-feature`)
5. Откройте Pull Request

## 📝 Лицензия

Этот проект лицензирован под MIT License - см. файл [LICENSE](LICENSE) для деталей.

## 🙏 Благодарности

-   [Vue.js](https://vuejs.org/) - За отличный фреймворк
-   [OpenWeatherMap](https://openweathermap.org/) - За API погоды
-   [Heroicons](https://heroicons.com/) - За красивые иконки
-   [Vite](https://vitejs.dev/) - За быстрый инструмент сборки

## 📞 Поддержка

Если у вас есть вопросы или предложения, создайте [Issue](https://github.com/FrankFMY/vue3js-weather-app/issues) в репозитории.

---

⭐ **Если проект вам понравился, поставьте звездочку!**

