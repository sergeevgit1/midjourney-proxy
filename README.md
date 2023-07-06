# midjourney-proxy

Канал Discord с агентом MidJourney, реализующий использование искусственного интеллекта для рисования через API

[![GitHub release](https://img.shields.io/static/v1?label=release&message=v2.3.3&color=blue)](https://www.github.com/novicezk/midjourney-proxy)
[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)

## Текущий функционал
- [x] Поддержка команды Imagine и связанных операций U, V
- [x] При использовании команды Imagine можно добавить изображение в формате base64 в качестве фона.
- [x] Поддерживается команда Blend (смешивание изображений) и связанные операции U, V.
- [x] Поддерживается команда Describe, которая генерирует подсказку на основе изображения.
- [x] Поддерживается команда Imagine, V, Blend для отслеживания прогресса генерации изображений.
- [x] Поддерживается перевод китайского текста вводом команды "支持中文prompt翻译" с использованием сервиса Baidu Translate или GPT-3.5 Turbo.
- [x] Поддерживается определение чувствительных слов в prompt и возможность их настройки и Задача по умолчанию: очередь 10, одновременное выполнение 3. Можно настроить mj.queue, см. MidJourney уровни подписки.
- [x] Подключение через user-token к wss, позволяет получать информацию об ошибках и полный функционал.
- [x] Поддержка обратного прокси для домена Discord (server, cdn, wss), настройка mj.ng-discord.

## План на будущее:
- [ ] Поддержка операции Reroll
- [ ]  Поддержка настройки пула аккаунтов для распределения задач по рисованию
- [ ] Исправление связанных ошибок, Вики / Известные проблемы

## Использование предполагает следующее:
1. Зарегистрируйтесь в MidJourney и создайте свой собственный канал, см. Краткое руководство.
2. Получите токен пользователя, идентификатор сервера и идентификатор канала: Инструкции по получению.

## Предупреждение о рисках:
1. Частые действия по созданию изображений могут привести к предупреждению аккаунта MidJourney. Пожалуйста, будьте осторожны при использовании.
2. Для снижения риска, установите mj.discord.user-agent и mj.discord.session-id.
3. По умолчанию используется user-wss, что позволяет получать информацию об ошибках MidJourney и прогрессе изменения изображений, но может увеличить риск для аккаунта.
4. Поддерживается настройка mj.discord.user-wss в значение false для подключения через bot-token, требуется добавление пользовательского бота: [Инструкции](./docs/discord-bot.md)

## Развертывание на платформе Railway
Развертывание на основе платформы Railway не требует собственного сервера: Инструкция по развертыванию; если невозможно использовать Railway, можно воспользоваться развертыванием на платформе Zeabur, указанном ниже.

## Развертывание на платформе Zeabur
Развертывание на основе платформы Zeabur не требует собственного сервера: Инструкция по развертыванию

## Развертывание с использованием Docker
1. В директории /xxx/xxx/config создайте файлы application.yml (настройки mj) и banned-words.txt (опционально, для замены стандартного файла с запрещенными словами); см. файлы в директории src/main/resources в качестве примера.
2. Запустите контейнер, смонтировав директорию config:
```shell
docker run -d --name midjourney-proxy \
 -p 8080:8080 \
 -v /xxx/xxx/config:/home/spring/config \
 --restart=always \
 novicezk/midjourney-proxy:2.3.3
```
3. Откройте `http://ip:port/mj`, чтобы просмотреть документацию по API.

Примечание: Если не хотите монтировать директорию config, можно указать параметры прямо в команде запуска контейнера:
```shell
docker run -d --name midjourney-proxy \
 -p 8080:8080 \
 -e mj.discord.guild-id=xxx \
 -e mj.discord.channel-id=xxx \
 -e mj.discord.user-token=xxx \
 --restart=always \
 novicezk/midjourney-proxy:2.3.3
```

## Настройки
- mj.discord.guild-id: ID сервера Discord
- mj.discord.channel-id: ID канала Discord
- mj.discord.user-token: Токен пользователя Discord
- mj.discord.session-id: Идентификатор сеанса пользователя Discord, если не указан, используется значение по умолчанию. Рекомендуется скопировать и заменить его из запроса interactions.
- mj.discord.user-agent: User-Agent для вызова API Discord и подключения к wss. По умолчанию используется значение автора. Рекомендуется скопировать и заменить его из сетевых запросов браузера.
- mj.discord.user-wss: Использовать ли user-token для подключения к wss. По умолчанию true.
- mj.discord.bot-token: Пользовательский токен бота. Обязателен, если user-wss=false.
- Дополнительные настройки см. в [Wiki / Настройки](https://github.com/novicezk/midjourney-proxy/wiki/Настройки)

## Ссылки на Wiki:
1. [Описание API-интерфейса](https://github.com/novicezk/midjourney-proxy/wiki/Описание-API-интерфейса)
2. [Обратный вызов изменений задачи](https://github.com/novicezk/midjourney-proxy/wiki/Обратный-вызов-изменений-задачи)
3. [История обновлений](https://github.com/novicezk/midjourney-proxy/wiki/История-обновлений)

## Важно:
1. Общие вопросы и решения проблем смотрите в [FAQ](https://github.com/novicezk/midjourney-proxy/wiki/FAQ).
2. Задавайте другие вопросы или предложения в разделе [Issues](https://github.com/novicezk/midjourney-proxy/issues).
3. Заинтересованные пользователи также могут присоединиться к группе обсуждения. Сканируйте QR-код, чтобы присоединиться к группе. Места в группе уже заняты, но вы можете запросить приглашение у администратора через WeChat.
 <img src="https://raw.githubusercontent.com/novicezk/midjourney-proxy/main/docs/manager-qrcode.png" width="320" alt="微信二维码"/>

## Локальная разработка:
- Зависимости: требуется Java 17 и Maven.
- Изменение настроек: измените файл src/main/application.yml.
- Запуск проекта: запустите функцию main в классе ProxyApplication.
- После внесения изменений в код, создайте образ: раскомментируйте строку с VOLUME в Dockerfile и выполните команду `docker build . -t midjourney-proxy`.

## Приложения проекта:
- [wechat-midjourney](https://github.com/novicezk/wechat-midjourney): Прокси-клиент для WeChat, интегрируется с MidJourney. Это пример использования и больше не обновляется.
- [stable-diffusion-mobileui](https://github.com/yuanyuekeji/stable-diffusion-mobileui): SDUI - основан на этом API и SD, позволяет одним нажатием упаковать H5 и мини-приложения.
- [ChatGPT-Midjourney](https://github.com/Licoy/ChatGPT-Midjourney): Создайте свой собственный веб-сервис ChatGPT+Midjourney всего в один клик.
- [MidJourney-Web](https://github.com/ConnectAI-E/MidJourney-Web): 🍎 Наслаждайтесь улучшенным опытом MidJourney на веб-интерфейсе.
- Если ваш проект зависит от данного и является открытым, свяжитесь с автором, чтобы добавить его сюда.

## Другое:
Если этот проект был полезен для вас, пожалуйста, поставьте звезду (star); вы также можете угостить автора чашкой чая～

 <img src="https://raw.githubusercontent.com/novicezk/midjourney-proxy/main/docs/receipt-code.png" width="220" alt="QR-код"/>

![График истории звезд](https://api.star-history.com/svg?repos=novicezk/midjourney-proxy&type=Date)