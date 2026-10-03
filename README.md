# Структура:

# AI-Column
```text
├── firmware/
│   ├── main.ino (полная прошивка)
│   ├── platformio.ini
│   ├── include/
│   │   ├── audio.h
│   │   ├── display.h
│   │   ├── api.h
│   │   └── config.h
│   ├── src/
│   │   ├── audio.cpp
│   │   ├── display.cpp
│   │   ├── api.cpp
│   │   └── main.cpp
│   └── README.md (как прошить)
│
├── cloud/
│   ├── server.js (Node.js)
│   ├── package.json
│   ├── routes/
│   │   ├── audio.js
│   │   ├── commands.js
│   │   └── sync.js
│   ├── models/
│   │   ├── User.js
│   │   ├── Command.js
│   │   └── Audio.js
│   └── README.md (запуск облака)
│
├── hardware/
│   ├── schematic.kicad (схема)
│   ├── pcb/ (PCB дизайн)
│   ├── 3d-model/ (корпус в Fusion 360)
│   └── BOM.csv (список компонентов)
│
├── docs/
│   ├── ARCHITECTURE.md (архитектура)
│   ├── API.md (REST API)
│   ├── HARDWARE.md (железо)
│   ├── SETUP.md (настройка)
│   └── ROADMAP.md (планы)
│
├── ui/
│   ├── screens/ (дизайн экранов)
│   ├── icons/ (иконки RGB LED)
│   └── fonts/ (шрифты для TFT)
│
├── tests/
│   ├── test_audio.cpp
│   ├── test_api.cpp
│   └── test_cloud.js
│
├── marketing/
│   ├── README.md
│   ├── pitch.pdf
│   ├── demo_video_script.md
│   └── social_media.md
│
├── .github/
│   ├── workflows/
│   │   ├── ci.yml (автотесты)
│   │   └── build.yml
│   └── ISSUE_TEMPLATE/
│
├── README.md (главный файл проекта)
├── LICENSE (MIT)
├── CONTRIBUTING.md (как контрибьютить)
└── .gitignore
```

# 📁 СТРУКТУРА РЕПОЗИТОРИЯ JARVIS

## `/firmware` - Прошивка для ESP32-S3

**ДЛЯ ЧЕГО:** Код микроконтроллера, который запускается на устройстве

- **`main.ino`** - точка входа, первый файл при включении
- **`platformio.ini`** - конфиг сборки (какой board, какие библиотеки, порты)
- **`include/`** - заголовочные файлы (объявления функций)
  - **`audio.h`** - функции для работы с микрофоном INMP441
  - **`display.h`** - функции для работы с TFT экраном ILI9341
  - **`api.h`** - функции для HTTP запросов к Claude/Whisper/TTS
  - **`config.h`** - константы (пины GPIO, WiFi, API ключи)
- **`src/`** - реализация функций (основной код)
  - **`audio.cpp`** - код работы с микрофоном (I2S протокол)
  - **`display.cpp`** - код работы с экраном (SPI протокол)
  - **`api.cpp`** - код для HTTP запросов
  - **`main.cpp`** - главная логика программы
- **`README.md`** - инструкция как установить IDE и прошить

---

## `/cloud` - Облачный сервер (Node.js)

**ДЛЯ ЧЕГО:** Backend сервер для сохранения команд, синхронизации, API для приложений

- **`server.js`** - главный файл сервера (запуск на Heroku или Hetzner)
- **`package.json`** - зависимости проекта (express, mongoose, cors, dotenv)
- **`routes/`** - API endpoints (маршруты которые вызывает firmware)
  - **`audio.js`** - POST /api/upload-audio (загрузка аудио)
  - **`commands.js`** - GET /api/commands (история команд пользователя)
  - **`sync.js`** - GET /api/sync (синхронизация между устройствами)
- **`models/`** - схемы базы данных MongoDB
  - **`User.js`** - схема пользователя (email, userId, apiKey, devices)
  - **`Command.js`** - схема команды (что сказал, ответ, время, duration)
  - **`Audio.js`** - схема аудиофайла (путь, размер, длина, метаданные)
- **`README.md`** - инструкция как запустить облако локально и на Heroku

---

## `/hardware` - Электроника и механика

**ДЛЯ ЧЕГО:** Схемы, PCB дизайн, 3D модель корпуса

- **`schematic.kicad`** - электрическая схема в KiCad (какой пин куда подключить)
- **`pcb/`** - печатная плата
  - **`gerber/`** - Gerber файлы (отправляешь на JLCPCB для производства)
  - **`layout/`** - размещение компонентов на плате
- **`3d-model/`** - механический дизайн корпуса в Fusion 360
  - **`jarvis-desk.step`** - CAD модель (редактируется в Fusion 360)
  - **`jarvis-desk.stl`** - STL файл для 3D принтера
  - **`parts/`** - отдельные части (крышка, дно, полки, монтажи)
- **`BOM.csv`** - Bill of Materials (список всех компонентов: ESP32, INMP441, динамик и т.д.)

---

## `/docs` - Документация

**ДЛЯ ЧЕГО:** Подробная документация для разработчиков

- **`ARCHITECTURE.md`** - архитектура: как микрофон → ESP32 → облако → динамик
- **`API.md`** - REST API документация (endpoints, параметры, примеры curl запросов)
- **`HARDWARE.md`** - описание каждого компонента и его GPIO пинов
- **`SETUP.md`** - как начать разработку (установка инструментов, зависимостей)
- **`ROADMAP.md`** - план развития на год (какие фичи будут добавлены)

---

## `/ui` - Интерфейсы и визуальные элементы

**ДЛЯ ЧЕГО:** Дизайн экранов, иконки, шрифты

- **`screens/`** - дизайн TFT экрана (2.4" ILI9341)
  - **`listening.png`** - экран "слушаю..." (синий LED, анимация волны)
  - **`thinking.png`** - экран "думаю..." (красный LED, прогресс-бар)
  - **`speaking.png`** - экран "говорю..." (зелёный LED, волна звука)
- **`icons/`** - маленькие иконки для индикаторов
  - **`micro.png`** - иконка микрофона
  - **`wifi.png`** - иконка WiFi сигнала
  - **`battery.png`** - иконка батареи
- **`fonts/`** - шрифты для текста на экране
  - **`arial.ttf`** - стандартный шрифт Arial

---

## `/tests` - Автоматические тесты

**ДЛЯ ЧЕГО:** Проверка качества кода, CI/CD тесты

- **`test_audio.cpp`** - тесты функций записи и воспроизведения аудио
- **`test_api.cpp`** - тесты HTTP запросов к облаку
- **`test_cloud.js`** - тесты Node.js endpoints (POST/GET запросы)

---

## `/marketing` - Маркетинг и промоция

**ДЛЯ ЧЕГО:** Подготовка к запуску, привлечение пользователей

- **`README.md`** - маркетинг стратегия и план продвижения
- **`pitch.pdf`** - презентация для инвесторов (elevator pitch, финансовая модель)
- **`demo_video_script.md`** - сценарий видео для YouTube (что показывать, что говорить)
- **`social_media.md`** - посты в Twitter/LinkedIn/Telegram для раскрутки проекта

---

## `/.github` - GitHub Actions (автоматизация)

**ДЛЯ ЧЕГО:** Автоматические тесты при каждом push

- **`workflows/`** - рабочие потоки GitHub Actions
  - **`ci.yml`** - Continuous Integration (тесты запускаются при git push)
  - **`build.yml`** - сборка прошивки и проверка на ошибки
- **`ISSUE_TEMPLATE/`** - шаблоны для issues (баги, фичи, вопросы)

---

## Корневые файлы проекта

- **`README.md`** - главная страница проекта (первое что видят)
  - Что такое JARVIS
  - Быстрый старт (как установить, как запустить)
  - Ссылки на видео, документацию
  - Как помочь проекту (контрибьютинг)

- **`LICENSE`** - MIT лицензия (открытый исходный код, можно использовать свободно)

- **`CONTRIBUTING.md`** - инструкция для контрибьютеров
  - Как форкнуть проект
  - Как создать ветку
  - Как сделать Pull Request
  - Код стиль и стандарты

- **`.gitignore`** - файлы которые НЕ коммитить в GitHub
  - `node_modules/` - зависимости npm
  - `.pio/` - кэш PlatformIO
  - `.env` - секретные API ключи
  - `*.o, *.so` - скомпилированные файлы
  - `.DS_Store` - файлы macOS

---

## 🎯 СТРУКТУРА В ДЕТАЛЯХ

### Как это работает вместе:

```
FRONTEND (на устройстве):
firmware/src/main.cpp → запускается на ESP32-S3
                    ↓ (читает)
                    firmware/include/config.h (пины и константы)
                    ↓ (использует)
                    firmware/src/audio.cpp (микрофон)
                    firmware/src/display.cpp (экран)
                    firmware/src/api.cpp (HTTP запросы)
                    ↓ (отправляет команду)
                    
BACKEND (в облаке):
cloud/server.js → запускается на Heroku
            ↓ (получает)
            cloud/routes/commands.js (POST /api/upload-command)
            ↓ (сохраняет в)
            cloud/models/Command.js (MongoDB)
            ↓ (обработка и ответ)
            cloud/routes/sync.js (GET /api/sync)

ПОДДЕРЖКА:
docs/ARCHITECTURE.md → объясняет как это работает
docs/API.md → как использовать endpoints
hardware/schematic.kicad → какие пины куда подключены
ui/screens/*.png → как выглядит экран
tests/*.js → проверяют что всё работает
```

---

## 📊 РАЗДЕЛЕНИЕ РАБОТЫ

| Папка | Кто работает | Что делает |
|-------|--------------|-----------|
| `/firmware` | Embedded инженер | C++ код для ESP32 |
| `/cloud` | Backend разработчик | Node.js сервер и API |
| `/hardware` | Hardware инженер | KiCad схема, PCB, 3D модель |
| `/tests` | QA тестер | Автотесты |
| `/ui` | UI/UX дизайнер | Дизайн экранов и иконок |
| `/docs` | Техписатель | Документация |
| `/marketing` | PM + Маркетер | Видео, посты, презентации |

**Все могут работать ОДНОВРЕМЕННО и независимо друг от друга!** ✅

---

## ✅ ГЛАВНЫЙ ПРИНЦИП

```
DRY = Don't Repeat Yourself

❌ ПЛОХО:
Код микрофона в main.cpp
Код экрана в main.cpp  
Код API в main.cpp
→ 1000 строк в одном файле, непонятно, нельзя переиспользовать

✅ ХОРОШО:
audio.h + audio.cpp (только микрофон)
display.h + display.cpp (только экран)
api.h + api.cpp (только API)
main.cpp (собирает всё вместе)
→ Каждый файл отвечает за одно, переиспользуется, понятно
```
