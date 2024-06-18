# Task 3.38 Compose. Запуск compose со сборкой образа

### Description


Используемые технологии:  Linux, Docker, Docker-Compose

Еще один приятный кейс использования Docker Compose - это быстрый запуск приложения, которое находится в разработке, но еще не выложено в образ.

Для этого есть возможность собирать наше приложение при помощи директивы build.

```
Пример использования:
version: "3"
services:
  redis:
    image: redis:4-alpine

  app:
    build: .
    environment:
      REDIS_HOST: redis
      REDIS_PORT: 6379
```

Для запуска же приложений нужно выполнить 2 команды:
```
docker-compose build
docker-compose up -d
```
Но на этом мы еще не закончили. Директива build: . - это простейшая форма, как у тех же volumes и networks из директив, рассмотренных ранее.

Полная форма имеет более гибкие возможности:
```
version: "3"
services:
  app:
    build:
      context: ./app/
      dockerfile: Dockerfile-alternate
      args:
        buildno: 1
      cache_from:
        - app:latest
```

В целом, с учетом разобранного материала, каждый параметр отдельно разъяснять не надо, поэтому лучше разберем более интересные особенности.

В случае, если мы укажем вместе с build еще и image, то в ходе выполнения docker-compose build собранный образ будет сохранен именно с тем именем и тегом, который мы указали в image.

Напоследок расскажем еще один лайфхак - если у нас есть несколько окружений для приложений, можно использовать несколько docker-compose файлов, которые будут соединены в один при выполнении.

К примеру, если у вас есть 3 файла:
```
$ cat docker-compose.base.yml
version: "3"
services:
  redis:
    image: redis:4-alpine

  app:
    environment:
      REDIS_HOST: redis
      REDIS_PORT: 6379
```
```      
$ cat docker-compose.build.yml
services:
  app:
    build: .
    image: app:local
```
```    
$ cat docker-compose.deploy.yml
services:
  app:
    image: app:prod
```
То можно запускать сервис во время разработки при помощи команд:
```
docker-compose -f docker-compose.base.yml -f docker-compose.build.yml build
docker-compose -f docker-compose.base.yml -f docker-compose.build.yml up -d
```

А для выкладки стабильного образа в registry:
`docker-compose -f docker-compose.base.yml -f docker-compose.deploy.yml push`

## Чекпоинты:

Склонируй репозиторий voting в свой аккаунт на devops-gitlab.inno.ws с именем dkr-30-voting.

Напиши docker-compose.yml файл, который бы собирал приложение, запускал его и все требуемые зависимости:

- nginx - проксирует запросы на voting и доступен на хосте на порту 20000. Пример конфигурационного файла находится в папке nginx, смонтируй его в /etc/nginx/conf.d/default.conf. Используй alpine-версию образа.
- voting - собирается из репозитория;
- mysql - база данных, к которой подключается voting;
- redis - In-memory хранилище для кэша;

Запусти сервис с именем проекта inno30.

Сконфигурируй приложение, выполнив команды из раздела Migration и Seeding в README репозитория.

Обратись к сервису по localhost:20000/polls (в ответе ты должен увидеть json-объект).
Загрузи новые файлы в репозиторий