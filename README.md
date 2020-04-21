# Smart weather bot

Бота можно найти в Telegram по юзернейму [@smweatherbot](https://t.me/smweatherbot)

## Умный сервис прогноза погоды

Smart weather bot - Telegram-бот, уведомляющий вас о погоде в выбранной локации. Также он посоветует, что надеть в зависимости от погоды и сможет каждый день в определенное время отправлять вам погоду.   
Проект выполнен в рамках тестового задания (**уровень сложности - задача со звездочкой**) для "Школы будущих СТО" Яндекс.Облака и Phoenix Education.

## Проектирование

- Используемые технологии: `Python 3`, `python-telegram-bot`, `OpenWeathermap API`, `Timezones DB API`
- Пользовательский интерфейс - чат-бот
- Формат ответа - текст. Данные, полученные с сервера, подставляются в текстовый шаблон (об этом в видео)

## Процесс работы

Сервис - чат-бот, который по локации (городу) пользователя отправляет ему актуальную информацию о погоде в этом городе.

- При старте пользователь вводит локацию (город)
  - Пользователь нажимает на кнопку "Показать погоду"
    - Формируется запрос и отправляется к `OpenWeather API`
    - Полученный ответ подставляется в текстовый шаблон
    - Отправляется пользователю
  - Пользователь нажимает на кнопку "Сменить локацию"
    - Механизм такой же, как и при старте
  - Пользователь нажимает на кнопку "Ежедневное оповещение о погоде"
    - Пользователь вводит время, в которое он хочет получать информацию
    - Для определения часового пояса формируется и отправляется запрос к `TimezonesDB API`
    - Устанавливается ежедневный таймер на отправку сообщения пользователю в соответствии с часовым поясом текущей локации.

## Некоторые нюансы

- Если установить ежедневное оповещение о погоде и сменить локацию, то на оповещение это не повлияет - оно по-прежнему будет установлено на тот город, на который устанавливалось и на тот часовой пояс соответственно.
- В качестве `TODO` я оставил сохранение таймеров в файл. То есть сейчас, если бот по какой-либо причине будет остановлен или перезапущен, то все таймеры на ежедневное оповещение сбросятся.
- Интерфейс бота реализован на английском языке, потому что так было удобнее работать с `OpenWeather API` и использовать список доступных городов, который они предлагают (он полностью на английском)
- В случае, если пользователь вводит название города и по его запросу было найдено несколько городов, пользователю будет отправляться подтверждение локации до тех пор, пока он не подтвердит нужную
- Локация - это именно город, потому что `OpenWeather API` рекомендует отправлять запросы, используя `City ID` из списка доступных городов.

## Как запустить бота локально

В файле `bot/config.py` нужно установить значения трех токенов:

- `BOT_TOKEN` - токен для бота Telegram
- `OPEN_WEATHER_TOKEN` - токен для [OpenWeather API](https://openweathermap.org/appid)
- `TIMEZONEDB_TOKEN` - токен для [TimezonesDB API](https://timezonedb.com/api)

Импорты в этом файле можно закомментировать, так как подгрузка переменных среды не потребуется.

Главный запускаемый файл - `main.py` в корневой папке проекта.

