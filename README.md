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

## Zeabur 部署
基于Zeabur平台部署，不需要自己的服务器: [部署方式](./docs/zeabur-start.md)

## Docker 部署
1. /xxx/xxx/config目录下创建 application.yml(mj配置项)、banned-words.txt(可选，覆盖默认的敏感词文件)；参考src/main/resources下的文件
2. 启动容器，映射config目录
```shell
docker run -d --name midjourney-proxy \
 -p 8080:8080 \
 -v /xxx/xxx/config:/home/spring/config \
 --restart=always \
 novicezk/midjourney-proxy:2.3.3
```
3. 访问 `http://ip:port/mj` 查看API文档

附: 不映射config目录方式，直接在启动命令中设置参数
```shell
docker run -d --name midjourney-proxy \
 -p 8080:8080 \
 -e mj.discord.guild-id=xxx \
 -e mj.discord.channel-id=xxx \
 -e mj.discord.user-token=xxx \
 --restart=always \
 novicezk/midjourney-proxy:2.3.3
```
## 配置项
- mj.discord.guild-id：discord服务器ID
- mj.discord.channel-id：discord频道ID
- mj.discord.user-token：discord用户Token
- mj.discord.session-id：discord用户的sessionId，不设置时使用默认的，建议从interactions请求中复制替换掉
- mj.discord.user-agent：调用discord接口、连接wss时的user-agent，默认使用作者的，建议从浏览器network复制替换掉
- mj.discord.user-wss：是否使用user-token连接wss，默认true
- mj.discord.bot-token：自定义机器人Token，user-wss=false时必填
- 更多配置查看 [Wiki / 配置项](https://github.com/novicezk/midjourney-proxy/wiki/%E9%85%8D%E7%BD%AE%E9%A1%B9)

## Wiki链接
1. [Wiki / API接口说明](https://github.com/novicezk/midjourney-proxy/wiki/API%E6%8E%A5%E5%8F%A3%E8%AF%B4%E6%98%8E)
2. [Wiki / 任务变更回调](https://github.com/novicezk/midjourney-proxy/wiki/%E4%BB%BB%E5%8A%A1%E5%8F%98%E6%9B%B4%E5%9B%9E%E8%B0%83)
2. [Wiki / 更新记录](https://github.com/novicezk/midjourney-proxy/wiki/%E6%9B%B4%E6%96%B0%E8%AE%B0%E5%BD%95)

## 注意事项
1. 常见问题及解决办法见 [Wiki / FAQ](https://github.com/novicezk/midjourney-proxy/wiki/FAQ) 
2. 在 [Issues](https://github.com/novicezk/midjourney-proxy/issues) 中提出其他问题或建议
3. 感兴趣的朋友也欢迎加入交流群讨论一下，扫码进群名额已满，加管理员微信邀请进群

 <img src="https://raw.githubusercontent.com/novicezk/midjourney-proxy/main/docs/manager-qrcode.png" width="320" alt="微信二维码"/>

## 本地开发
- 依赖java17和maven
- 更改配置项: 修改src/main/application.yml
- 项目运行: 启动ProxyApplication的main函数
- 更改代码后，构建镜像: Dockerfile取消VOLUME的注释，执行 `docker build . -t midjourney-proxy`

## 应用项目
- [wechat-midjourney](https://github.com/novicezk/wechat-midjourney) : 代理微信客户端，接入MidJourney，仅示例应用场景，不再更新
- [stable-diffusion-mobileui](https://github.com/yuanyuekeji/stable-diffusion-mobileui) : SDUI，基于本接口和SD，可一键打包生成H5和小程序
- [ChatGPT-Midjourney](https://github.com/Licoy/ChatGPT-Midjourney) : 一键拥有你自己的 ChatGPT+Midjourney 网页服务
- [MidJourney-Web](https://github.com/ConnectAI-E/MidJourney-Web) : 🍎 Supercharged Experience For MidJourney On Web UI
- 依赖此项目且开源的，欢迎联系作者，加到此处展示

## 其它
如果觉得这个项目对你有所帮助，请帮忙点个star；也可以请作者喝杯茶～

 <img src="https://raw.githubusercontent.com/novicezk/midjourney-proxy/main/docs/receipt-code.png" width="220" alt="二维码"/>

![Star History Chart](https://api.star-history.com/svg?repos=novicezk/midjourney-proxy&type=Date)
