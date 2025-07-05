#DockerCompose 

#### Хорошая шпаргалка по докеру есть [тут](https://github.com/eon01/DockerCheatSheet) и [тут](https://dker.ru/docs/docker-engine/engine-reference/command-line-reference/docker-commands/)

---
- [[#DOCKER RUN]]
- [[#ПРОСТЫЕ ДЕЙСТВИЯ С КОНТЕЙНЕРАМИ]]
- [[#ЗАПУСК И ОСТАНОВКА КОНТЕЙНЕРОВ]]
- [[#ИНФОРМАЦИЯ О КОНТЕЙНЕРЕ]]
- [[#РАБОТА С КОНТЕЙНЕРАМИ]]
- [[#РАБОТА С REGISTRY]]
- [[#РАБОТА С ОБРАЗАМИ]]
- [[#РАБОТА С ТОМАМИ]]
- [[#СЕТИ]]
- [[ЧИСТКА МУСОРА]]
---

## DOCKER RUN

```shell
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

```shell
[ OPTIONS ]

-it — интерактивный режим. Перейти в контейнер и запустить внутри контейнера команду
-d — запустить контейнер в фоне (демоном) и вывести его ID
-p port_localhost:port_docker_image — порты из докера на локалхост
-e «TZ=Europe/Moscow» — указываем нашему контейнеру timezone
-h HOSTNAME — присвоить имя хоста контейнеру
— link <имя контейнера> — связать контейнеры с другим
-v /local/path:/container/path/ — прокидываем в контейнер докера директорию с локальной машины
--name CONTAINERNAME — присвоить имя нашему контейнеру
--restart=[no/on-failure/always/unless-stopped] — варианты перезапуска контейнера при крэше
```

#### Подробнее про [опции запуска тут](https://docs.docker.com/engine/reference/commandline/run/)

```shell
# Для больше читабельности можно при переносе строки ставить символ \
docker run \ 
—restart=always -it \
```

---

## ПРОСТЫЕ ДЕЙСТВИЯ С КОНТЕЙНЕРАМИ

### Создание контейнера

```shell
docker create -t -i eon01/infinite --name <CONTAINERNAME or CONTAINERID>
```

### Переименование контейнера

```shell
docker rename <OLD CONTAINERNAME> <NEW CONTAINERNAME>
```

### Удаление контейнера

```shell
docker rename <OLD CONTAINERNAME> <NEW CONTAINERNAME>
```

### Обновление контейнера

```shell
docker update --cpu-shares 512 -m 300M <CONTAINERNAME or CONTAINERID>
```

---

## ЗАПУСК И ОСТАНОВКА КОНТЕЙНЕРОВ

### Запуск остановленного контейнера

```shell
docker start <CONTAINERNAME>
```

### Остановка

```shell
docker stop <CONTAINERNAME>
```

### Перезагрузка

```shell
docker restart <CONTAINERNAME>
```

### Пауза (приостановка всех процессов контейнера)

```shell
docker pause <CONTAINERNAME>
```

### Снятие паузы

```shell
docker unpause <CONTAINERNAME>
```

### Блокировка (до остановки контейнера)

```shell
docker wait <CONTAINERNAME>
```

### Отправка SIGKILL (завершающего сигнала)

```shell
docker kill <CONTAINERNAME>
```

### Отправка другого сигнала

```shell
docker kill -s HUP <CONTAINERNAME>
```

### Подключение к существующему контейнеру

```shell
docker attach <CONTAINERNAME>
```

Выход через Ctrl+p+q

---

## ИНФОРМАЦИЯ О КОНТЕЙНЕРЕ

### Работающие контейнеры

```shell
# Вывести работающие контейнеры
docker ps

# Вывести все контейнеры
docker ps -a
```

### Логи контейнера

```shell
docker logs <CONTAINERNAME or CONTAINERID>
```

### Информация о контейнере

```shell
# Все параметры контейнера
docker inspect <CONTAINERNAME or CONTAINERID>

# Вывести величину конкретного параметра
docker inspect --format '{{ .NetworkSettings.IPAddress }}' $(docker ps -q)
```

### События контейнера

```shell
docker events <CONTAINERNAME or CONTAINERID>
```

### Публичные порты

```shell
docker port <CONTAINERNAME or CONTAINERID>
```

### Выполняющиеся процессы

```shell
docker top <CONTAINERNAME or CONTAINERID>
```

### Использование ресурсов

```shell
docker stats <CONTAINERNAME or CONTAINERID>
```

### Изменения в файлах или директориях файловой системы контейнера

```shell
docker diff <CONTAINERNAME or CONTAINERID>
```

---

## РАБОТА С КОНТЕЙНЕРАМИ

### Зайти в уже запущенный контейнер.

(точнее выполнить команду внутри контейнера)

```shell
docker exec -it name_of_container /bin/bash
```

### Запустить контейнер и открыть в нём bash

```shell
docker run -it -d --name my_container CONTAINER_ID /bin/bash
```

### Копирование файлов внутрь контейнера.

```shell
docker cp some_files.conf docker_container:/home/docker/
```

При смене путей, можно копировать из контейнера.

---

## РАБОТА С REGISTRY

### Вход в реестр

```shell
# Вход на докерзаб
docker login

# Вход в WebUI локального registry
docker login localhost:8080
```

### Выход из реестра

```shell
# Докерхаб
docker logout

# Локальный registry
docker logout localhost:8080
```

### Поиск образа

```shell
# Простой поиск
docker search nginx

# Поиск с фильтрами
docker search nginx -- filter stars=3 --no-trunc busybox
```

### Pull (выгрузка из реестра) образа

```shell
# Выгрузка из Докерхаба
docker pull nginx

# Из локального registry
docker pull eon01/nginx localhost:5000/myadmin/nginx
```

### Push (загрузка в реестр) образа

```shell
# В докерхаб
docker push eon01/nginx

# В локальный registry
docker push eon01/nginx localhost:5000/myadmin/nginx
```

---

## РАБОТА С ОБРАЗАМИ

### Список образов

```shell
docker images
```

### Создание образов

(файл опциями сборки образа), учитывая что мы находимся в папке где лежит этот файл. Через ключ -t назначаем имя нашему образу. Точка в конце означает что Dockerfile лежит в текущей директории.

```shell
docker build -t my_docker .
```

```shell
docker build .
```

```shell
docker build github.com/creack/docker-firefox
```

```shell
docker build - < Dockerfile
```

```shell
docker build - < context.tar.gz
```

```shell
docker build -t eon/infinite .
```

```shell
docker build -f myOtherDockerfile .
```

```shell
curl example.com/remote/Dockerfile | docker build -f - .
```

### Удаление образа

```shell
docker rmi nginx
```

### Загрузка репозитория в tar (из файла или стандартного ввода)

```shell
docker load < ubuntu.tar.gz
```

```shell
docker load --input ubuntu.tar
```

### Сохранение образа в tar-архив

```shell
docker save busybox > ubuntu.tar
```

### Просмотр истории образа

```shell
docker history
```

### Создание образа из контейнера

```shell
docker commit nginx
```

### Тегирование образа

```shell
docker tag nginx eon01/nginx
```

### Push (загрузка в реестр) образа

```shell
docker push eon01/nginx
```

---

## РАБОТА С ТОМАМИ

### Немного теории:

- Если монтируем пустой том с хоста, а в контейнере уже есть файлы, то они скопируются в том
- Если монтируем с хоста том с файлами, то они окажутся в контейнере
- Если мы монтируем не пустой том с хоста, а в контейнере по этому пути уже есть файлы, то они будут скрыты
- Можно монтировать с хоста любые файлы. В том числе и служебные. Например сокет docker. Что бы получился docker-in-docker (dind)

### Создание тома

```shell
docker volume create <VOLUME-NAME>
```

### Монтируем с хоста в контейнер

```shell
docker run --mount source=<VOLUME-NAME>,target=/path/to/folder/in/container -d <IMAGE>
```

### Монтируем с контейнера на хост

```shell
docker run --mount type=bind,source=/host/folder,target=/container/folder -d <IMAGE>
```

### Посмотреть настройки тома

```shell
docker volumes inspect <VOLUME-NAME>
```

### Вывести список всех томов с их названиями.

```shell
docker volume ls
```

### Удаление volumes по названию

```shell
docker volume rm <VOLUME-NAME>
```

---

## СЕТИ

### Создание сети

```shell
docker network create -d overlay MyOverlayNetwork
```

```shell
docker network create -d bridge MyBridgeNetwork
```

```shell
docker network create -d overlay \
  --subnet=192.168.0.0/16 \
  --subnet=192.170.0.0/16 \
  --gateway=192.168.0.100 \
  --gateway=192.170.0.100 \
  --ip-range=192.168.1.0/24 \
  --aux-address="my-router=192.168.1.5" --aux-address="my-switch=192.168.1.6" \
  --aux-address="my-printer=192.170.1.5" --aux-address="my-nas=192.170.1.6" \
  MyOverlayNetwork
```

### Удаление сети

```shell
docker network rm MyOverlayNetwork
```

### Список сетей

```shell
docker network ls
```

### Получение информации о сети

```shell
docker network inspect MyOverlayNetwork
```

### Подключение работающего контейнера к сети

```shell
docker network connect MyOverlayNetwork nginx
```

### Подключение контейнера к сети при его запуске

```shell
docker run -it -d --network=MyOverlayNetwork nginx
```

### Отключение контейнера от сети

```shell
docker network disconnect MyOverlayNetwork nginx
```

---

## ЧИСТКА МУСОРА

### Показать образы и контейнеры

```shell
# Вывести все образы (images)
docker images -a

# Вывести все неиспользуемые образы
docker images -f dangling=true

# Выведет список запущенных контейнеров, если добавить ключ -a, выведет список всех контейнеров.
docker ps

```

### Удаление образов (images)

```shell
# Для удаления используется команда docker rmi с добавлением ИД или тега, например:
# с ключом --force удалит контейнер и образ
docker rmi abb461727af5

# Удаление всех образов
docker rmi $(docker images -a -q)

# Удаление всех неиспользуемых образов
docker images prune
docker rmi $(docker images -f dangling=true -q)

# Удаление всех неиспользуемых (не связанных с контейнерами) образов:
# Если добавить к команде ключ -a, то произойдет удаление всех остановленных контейнеров и неиспользуемых образов.
docker system prune

# Удаление всех образов без тегов
docker rmi -f $(docker images | grep "^<none>" | awk "{print $3}")

# Удаление всех образов
docker rmi $(docker images -a -q)
```

### Удаление контейнеров (containers)

```shell
# Для удаления контейнера, его необходимо сначала остановить командой ниже с указанием ID или названия контейнера.
docker stop CONTAINER_ID

# Для удаления контейнера используется команда docker rm с добавлением ИД или названия
docker rm CONTAINER_ID

# Удаление контейнера и его тома (volume)
docker rm -v CONTAINER_ID

# Удаление всех контейнеров со статусом exited
docker rm $(docker ps -a -f status=exited -q)

# Удаление всех остановленных контейнеров
docker container prune
docker rm `docker ps -a -q`

# Удаление контейнеров, остановленных более суток назад
docker container prune --filter "until=24h"

# Остановка и удаление всех контейнеров
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)
```

### Удаление томов (volumes)

```shell
#Вывести список всех томов с их названиями.
docker volume ls

# Удаление volumes по названию
docker rm <volume_name>

# Вывести список всех томов не связанных с контейнерами
docker volume ls -f dangling=true

# Удаление томов (volumes) несвязанных с контейнерами
docker volume prune
docker volume rm $(docker volume ls -f dangling=true -q)

# Удаление неиспользуемых (dangling) томов по фильтру
docker volume prune --filter "label!=keep"
```

### Удаление сетей (networks)

```shell
# Вывести список всех сетей с их ИД и названиями. 
docker network ls

# Для удаления используется команда  с добавлением ИД или названия:
docker network rm NETWORK_ID

# Удалит все сети не используемые хотя бы одним контейнером.
docker network prune
```

### Удаление всех неиспользуемых объектов

```shell
docker system prune

# По умолчанию для Docker 17.06.1+ тома не удаляются. Чтобы удалились и они тоже:
docker system prune --volumes
```


---


