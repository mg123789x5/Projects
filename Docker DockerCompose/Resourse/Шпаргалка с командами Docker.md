#DockerCompose 

## *Распилить монолит на микросервисы — полная шпаргалка по работе с Docker*
## Docker Daemon и CLI

Работу Docker обеспечивают две центральные подсистемы:

- Docker Daemon — сервер, работающий в фоновом режиме. Он слушает запросы от CLI и управляет жизненным циклом контейнеров.

- Docker CLI — интерфейс командной строки Docker. Через строку отдаётся команда на поднятие, запуск или остановку контейнеров.

Docker CLI передаёт команды Docker Daemon выполнить конкретную команду. Причем CLI можно как установить на хост системе, так и настроить удалённый доступ — общение с Daemon происходит по REST API.

Функционал Docker Daemon не ограничивается запуском или остановкой контейнеров: система регулирует в том числе сети и порты, логирует контейнеры. Ниже приведу самые частотные команды к Docker Daemon.

### Шпаргалка по Docker CLI

Основные команды:

```
# запуск контейнера на основе указанного образа
docker run <имя_образа> 

# показать список активных контейнеров
docker ps 

# показать все контейнеры, включая остановленные
docker ps -a 

# остановить контейнер
docker stop <идентификатор_контейнера> 

# удалить контейнер
docker rm <идентификатор_контейнера> 

# показать список всех локальных образов
docker images 

# загрузить образ из Docker Hub
docker pull <имя_образа> 

# удалить локальный образ
docker rmi <идентификатор_образа>
```

Создание и работа с образами:

```
# сборка образа на основе Dockerfile
docker build -t <имя_образа>:<тег> <путь_к_Dockerfile> 

# пометить образ новым тегом
docker tag <старый_тег> <новый_тег> 

# переименовать и пометить образ для загрузки в другой репозиторий
docker tag <имя_образа>:<старый_тег> <новый_репозиторий>/<новый_тег> 

# отправка образа в Docker Hub или другой реестр
docker push <имя_репозитория>/<имя_образа>:<тег>
```

Сети и порты:

```
# показать список сетей
docker network ls

# определить соответствие портов при запуске контейнера
docker run -p <локальный_порт>:<контейнерный_порт> <имя_образа>
```

Работа с Docker Compose:

```
# запустить сервисы, определенные в файле `docker-compose.yml`
docker-compose up 

#остановить и удалить сервисы, описанные в файле `docker-compose.yml`
docker-compose down
```

Работа с Docker Volumes:

```
# создать Docker Volume
docker volume create <имя_volume> 

# запустить контейнер, подключив Volume
docker run -v <имя_volume>:<путь_в_контейнере> <имя_образа>
```

Логирование и мониторинг:

```
# показать логи контейнера
docker logs <идентификатор_контейнера>

# отобразить статистику использования ресурсов контейнера
docker stats <идентификатор_контейнера>
```

### Dockerfile

Команды по созданию образа фиксируются в необработанном (raw) текстовом документе — Docker-файле:

```
ИНСТРУКЦИЯ аргумент(ы)
```

где `ИНСТРУКЦИЯ` — команда для Docker Daemon, а `аргумент(ы)` — сам аргумент или конкретные значения, которые передаются в `ИНСТРУКЦИЮ`.

> Инструкции нечувствительны регистру, однако их принято писать «капсом», чтобы визуально отличать от аргументов.

Инструкции объясняют, что Docker Daemon должен сделать до, во время или после запуска контейнера из образа.

Основные инструкции для Dockerfile

Пример Docker-файла для приложения на Python:

```
# Используем базовый образ с Python
FROM python:3.8

# Устанавливаем зависимости
RUN pip install flask

# Копируем исходный код в образ
COPY . /app

# Указываем рабочую директорию
WORKDIR /app

# Определяем команду для запуска приложения
CMD ["python", "app.py"]
```

### Образ Docker (Docker Image)

Для того, чтобы из Dockerfile создать образ и запустить контейнер, нужно:

1. Перейти в директорию, где лежит dockerfile.
2. Командой `docker build` создать образ из файла.
3. По необходимости, проверить образы командой `docker images`.
4. Запустить контейнер из образа командой `docker run`.

В работе с образами можно использовать теги, чтобы указывать версию образов. По умолчанию, Docker присваивает тег `latest` при сборке.

```
#пример сборки образа с явным указанием тега
docker build -t my-python-app:v1.0
```

Для отправки образа в реестр Docker Hub используются следующие команды:

```
docker tag my-python-app:v1.0 username/my-python-app:v1.0
docker push username/my-python-app:v1.0
```

Загрузка образа из реестра выполняется командой:

```
docker pull username/my-python-app:v1.0
```

Образы Docker статичны. А вот контейнеры — изменяемы. Чтобы «обновить» образ, можно запустить из него контейнер, внести изменения и сохранить состояние в новый образ. Делается это с использованием команды `docker commit`:

```
docker commit -m "Добавлены изменения" -a "Автор" container_id username/my-python-app:v1.1
```

> Образ Docker — стандартный формат, то есть с ним может работать Docker Daemon на любой платформе. Это позволяет безболезненно портировать проекты с одной системы на другую — контейнеры упаковываются в образы и переносятся. А изоляция всех зависимостей и компонентов внутри образа гарантирует, что проект точно «встанет» на целевую платформу с Docker без дополнительной настройки.

### Контейнер (Docker Container)

Контейнер — это работающий в изолированной среде экземпляр образа (инстанс). В один контейнер «упаковывается» один работающий серверный процесс.

Можно, конечно, поместить несколько процессов, хоть целый монолит — строгих ограничений со стороны инструмента нет. Но это считается ошибкой проектирования микросервисной архитектуры. Docker позволяет настраивать взаимодействие контейнеров с внешней средой и другими контейнерами, а также регулировать потребление ресурсов. Так что весомых причин пытаться уместить всё в одном нет.

#### Дополнительные возможности

Если необходимо, чтобы контейнер работал с собственным экземпляром данных, не изменяя оригинал, мы можем смонтировать директорию из хост-системы в сам контейнер. Делается это командой:

```
docker run -v /путь/к/хост-директории:/путь/в/контейнере имя_образа
```

Docker Volumes — хранилища, которые связываются с контейнером, но не привязываются к его жизненному циклу. Это означает, что любые данные, которые контейнер отправляет в Volumes, сохранятся даже если контейнер остановить или уничтожить.

```
# команда для создания Volume в контейнере
 docker run -v my_volume:/путь/в/контейнере имя_образа
```

Чтобы передать переменные окружения в контейнер, используется флаг `-e` совместно с командой `RUN`:

```
docker run -e MY_VARIABLE=value имя_образа
```

Контейнер может экспортировать порты для взаимодействия с «внешним миром». Это особенно актуально для веб-приложений, где порты могут использоваться для доступа к веб-серверу.

```
docker run -p 8080:80 имя_образа
```

Можно ввести ограничения на ресурсы, используемые контейнером, такие как объем оперативной памяти или количество ядер CPU.

```
docker run --memory 512m --cpus 0.5 имя_образа
```

## Реестр Docker (Docker Registry)

Реестр Docker (Docker Registry) — это общедоступный репозиторий образов. Сервис помогает:

- централизованно хранить образы и их версии;
- ускорять развертывание — образы скачиваются сразу на целевую систему и готовы к работе;
- автоматизировать процессы сборки, тестирования и развертывания контейнеров.

Docker Hub — публичный реестр, в котором хранятся общедоступные образы (дистрибутивов Linux, баз данных, языков и прочее). Организации могут создавать собственные приватные реестры Docker, чтобы хранить конфиденциальные данные.

### Создание приватного реестра Docker

#### Установка Docker Distribution

Docker Distribution — это официальная реализация протокола Docker Registry. Установим его на сервер, который будет служить приватным реестром.

```
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```

Эта команда запускает приватный реестр на порту 5000. Опционально можно настроить HTTPS с использованием SSL-сертификата.

#### Использование Приватного Реестра

Теперь можно использовать реестр для хранения и распространения приватных Docker-образов.

```
# помечаем образ   
docker tag my-image localhost:5000/my-image
# отправляем образ в приватный реестр
docker push localhost:5000/my-image
```

## Запускаем монолит на сервере без Docker

В README приложения можно найти подробную инструкцию как развернуть его на сервере. Для примера возьмём README монолитного приложения, который мы намеренно сократили.

Установка пакетов:

```
sudo apt-get install -y build-essential libssl-dev zlib1g-dev libbz2-dev \
       libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
       xz-utils tk-dev libffi-dev liblzma-dev python-openssl git npm redis-server vim ffmpeg
```

Установка pyenv:

```
$ curl https://pyenv.run | bash
$ echo 'export PATH="$HOME/.pyenv/bin:$PATH"' >> ~/.bashrc && \
echo 'eval "$(pyenv init -)"' >> ~/.bashrc && \
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc && source ~/.bashrc
```

Установка Python 3.6.9:

```
pyenv install 3.6.9
```

Если установлена версия Ubuntu 20+ и возникла ошибка, применить следующие команды:

```
$ sudo apt install clang -y
$ CC=clang pyenv install 3.6.9
```

Создание виртуального окружения:

```
$ pyenv virtualenv 3.6.9 cpa-project
```

Активация виртуального окружения:

```
$ pyenv activate cpa-project
```

Установка NodeJS 8.11.3:

```
$ npm i n -g
$ sudo n install 8.11.3
$ sudo n # в появившемся окне выбираем версию 8.11.3
```

Клонирование проекта:

```
$ git clone git@github.com:User/cpa-project.git
```

Переходим в проект:

```
$ cd cpa-project
```

Установка зависимостей python (убедитесь, что виртуальное окружение активно):

```
$ pip install -U pip
$ pip install -r requirements.txt
```

Установка зависимостей nodejs

```
$ npm install
```

Проводим миграции для базы данных и создаем тестовые данные:

```
$ python manage.py migrate
$ python manage.py generate_test_data
```

Сборка клиентской части:

```
$ npm run watch # Для разработки с автоматическим ребилдом
$ npm run build # Для продакшена
```

## Собираем тот же проект в Docker

Давайте посмотрим, как это же приложение можно развернуть в среде Docker. Для начала выделим сервисы для compose-файла и придумаем им названия:

```
yaml
version: '3'
services:
 {stage}-project-ex--app:
   container_name: {stage}-project-ex--app
   build:
     context: ..
     dockerfile: Dockerfile
   env_file:
     - ".env.{stage}"
   networks:
     - stage_project-ex_network
   depends_on:
     - {stage}-project-ex--redis
     - {stage}-project-ex--clickhouse
     - {stage}-project-ex--postgres
     - {stage}-project-ex--mailhog
   volumes:
     - ..:/app/
     - ./crontab.docker:/etc/cron.d/crontab.docker
   command: /start
   labels:
     - "traefik.enable=true"
     - "traefik.http.routers.{stage}_fp_app.rule=Host(`web.{stage}.project-ex.io`)"
     - "traefik.http.services.{stage}_fp_app.loadbalancer.server.port=8000"
     - "traefik.http.routers.{stage}_fp_app.entrypoints=websecure"
     - "traefik.http.routers.{stage}_fp_app.tls.certresolver=stage_project-ex_app"

 {stage}-project-ex--app-cron:
   container_name: {stage}-project-ex--app-cron
   build:
     context: ..
     dockerfile: Dockerfile
   env_file:
     - ".env.{stage}"
   networks:
     - stage_project-ex_network
   depends_on:
     - {stage}-project-ex--redis
     - {stage}-project-ex--clickhouse
     - {stage}-project-ex--postgres
     - {stage}-project-ex--mailhog
   volumes:
     - ..:/app/
     - ./crontab.docker:/etc/cron.d/crontab.docker
   command: sh -c "printenv >> /etc/environment && crontab /etc/cron.d/crontab.docker && cron -f"

 {stage}-project-ex--front:
   container_name: {stage}-project-ex--front
   build: ./frontend-builder
   env_file:
     - ".env.{stage}"
   networks:
     - stage_project-ex_network
   depends_on:
     - {stage}-project-ex--app
   volumes:
     - ..:/app/

 {stage}-project-ex--clickhouse:
   container_name: {stage}-project-ex--clickhouse
   image: yandex/clickhouse-server:20.4.6.53
   env_file:
     - ".env.{stage}"
   networks:
     - stage_project-ex_network
   volumes:
     - /home/project-ex/stands/{stage}/docker_data/clickhouse/data:/var/lib/clickhouse
     - ./docker_data/clickhouse/schema:/var/lib/clickhouse/schema
     - ./docker_data/clickhouse/users.xml:/etc/clickhouse-server/users.xml
     - ./docker_data/clickhouse/project-ex.xml:/etc/clickhouse-server/users.d/default-user.xml
   labels:
     - "traefik.enable=true"
     - "traefik.tcp.routers.{stage}_fp_clickhouse.rule=HostSNI(`*`)"
     - "traefik.tcp.routers.{stage}_fp_clickhouse.entryPoints=clickhouse"
     - "traefik.tcp.routers.{stage}_fp_clickhouse.service={stage}_fp_clickhouse"
     - "traefik.tcp.services.{stage}_fp_clickhouse.loadbalancer.server.port=8123"

 {stage}-project-ex--postgres:
   container_name: {stage}-project-ex--postgres
   image: postgres:13.11-alpine
   env_file:
     - ".env.{stage}"
   networks:
     - stage_project-ex_network
   stdin_open: true
   tty: true
   volumes:
     - {stage}-project-ex--postgres:/var/lib/postgresql
   labels:
     - "traefik.enable=true"
     - "traefik.tcp.routers.postgres.rule=HostSNI(`*`)"
     - "traefik.tcp.routers.postgres.entryPoints=postgres"
     - "traefik.tcp.routers.postgres.service=postgres"
     - "traefik.tcp.services.postgres.loadbalancer.server.port=5432"

 {stage}-project-ex--redis:
   container_name: {stage}-project-ex--redis
   image: redis:alpine
   env_file:
     - ".env.{stage}"
   networks:
     - stage_project-ex_network
   volumes:
     - {stage}-project-ex--redis:/data

 {stage}-project-ex--mailhog:
   container_name: {stage}-project-ex--mailhog
   image: mailhog/mailhog:v1.0.1
   env_file:
     - ".env.{stage}"
   networks:
     - stage_project-ex_network
   labels:
     - "traefik.enable=true"
     - "traefik.http.routers.{stage}_fp_mailhog.rule=Host(`mail.{stage}.project-ex.io`)"
     - "traefik.http.services.{stage}_fp_mailhog.loadbalancer.server.port=8025"
     - "traefik.http.routers.{stage}_fp_mailhog.entrypoints=websecure"
     - "traefik.http.routers.{stage}_fp_mailhog.tls.certresolver=stage_project-ex_app"

volumes:
 {stage}-project-ex--postgres:
   name: {stage}-project-ex--postgres
   driver: local
 {stage}-project-ex--redis:
   name: {stage}-project-ex--project-ex
   driver: local

networks:
 stage_project-ex_network:
   external: true
   name: stage_project-ex_network
```

> Docker Compose — инструмент для запуска многоконтейнерных приложений в Docker. В .yaml-файле указываются все необходимые настройки и команды. Запуск контейнеров из compose-файла производится командой docker-compose up.

В .yaml-файле мы можем увидеть из каких контейнеров `container_name` (и каких версий) запускается наше ранее монолитное приложение. А ключевое слово `{stage}` — это ветка в GitLab, из которой будет подниматься контейнер. По желанию, на одном сервере мы можем запускать контейнеры из разных веток.

> От того, что мы разбили приложение на микросервисы, оно не перестало быть монолитным. Микросервисность продукта закладывается на стадии его проектирования и создания, когда каждая задача выделяется в отдельный сервис.

Строка в compose `dockerfile: Dockerfile` собирает наш контейнер. В файле содержатся такие инструкции:

```
dockerfile
FROM python:3.6.9-buster

ENV DJANGO_SETTINGS=advgame.local_settings

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
 # dependencies for building Python packages
 && apt-get install -y build-essential \
 # psycopg2 dependencies
 && apt-get install -y libpq-dev \
 # Translations dependencies
 && apt-get install -y gettext \
 # Cron
 && apt-get install -y cron \
 # Vim
 && apt-get install -y vim \
 # cleaning up unused files
 && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
 && rm -rf /var/lib/apt/lists/*

# Set timezone
ENV TZ=Europe/Moscow
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Have to invalidate cache here because Docker is bugged and doesn't invalidate cache
# even if requirements.txt did change

ADD ../requirements.txt /requirements.txt
RUN pip install -r /requirements.txt

COPY ./docker-compose/start.sh /start
RUN chmod +x /start

# Copy hello-cron file to the cron.d directory
COPY ./docker-compose/crontab.docker /etc/cron.d/crontab.docker
# Give execution rights on the cron job
RUN chmod 0644 /etc/cron.d/crontab.docker
# Apply cron job
RUN crontab /etc/cron.d/crontab.docker

COPY . /app

WORKDIR /app
```

Далее мы написали make-файл который позволит нам управлять конфигурированием проекта:

```
make
dir=${CURDIR}
project=project-ex
env=local
interactive:=$(shell [ -t 0 ] && echo 1)
ifneq ($(interactive),1)
  optionT=-T
endif

uid=$(shell id -u)
gid=$(shell id -g)
# Команда для docker-compose exec
c=
# Параметр для docker-compose exec
p=

dc:
  @docker-compose -f ./docker-compose/$(env).yml --env-file=./docker-compose/.env.$(env) $(cmd)

compose-logs:
  @make dc cmd="logs" env="$(env)"

cp-env:
  [ -f ./docker-compose/.env.$(env) ] && echo ".env.$(env) file exists" || cp ./docker-compose/.env.example ./docker-compose/.env.$(env)
  sed -i "s/{stage}/$(env)/g" ./docker-compose/.env.$(env)
  @if [ "$(env)" = "local" ] ; then \
     sed -i "s/{domain}/ma.local/g" ./docker-compose/.env.$(env) ; \
  fi;
  @if [ "$(env)" = "dev" ] ; then \
     sed -i "s/{domain}/dev.project-ex.io/g" ./docker-compose/.env.$(env) ; \
  fi;

cp-yml:
  @if [ ! "$(env)" = "local" ] ; then \
     [ -f ./docker-compose/$(env).yml ] && echo "$(env).yml file exists" || cp ./docker-compose/stage.example.yml ./docker-compose/$(env).yml ; \
     sed -i "s/{stage}/$(env)/g" ./docker-compose/$(env).yml; \
  fi;

init:
  docker network ls | grep stage_project-ex_network > /dev/null || docker network create stage_project-ex_network
  @make cp-env
  @make cp-yml
  [ -f ./docker-compose/.env.$(env) ] && echo ".env.$(env) file exists" || cp ./docker-compose/.env.$(env).example ./docker-compose/.env.$(env)
  @make dc cmd="up -d"
  @make dc cmd="start $(env)-$(project)--postgres" env="$(env)"
  sleep 5 && cat ./docker-compose/docker_data/pgsql/data/init_dump.sql | docker exec -i $(env)-$(project)--postgres psql -U project-ex
  @make dc cmd="exec $(env)-$(project)--app python ./manage.py migrate" env="$(env)"
  @make ch-restore env="$(env)"
  @make build-front env="$(env)"
  @make collect-static env="$(env)"

create_test_db:
  @make dc cmd="exec $(env)-$(project)--postgres dropdb --if-exists -U project-ex project-ex_test" env="$(env)" > /dev/null
  @make dc cmd="exec $(env)-$(project)--postgres createdb -U project-ex project-ex_test" env="$(env)"
  cat ./docker-compose/docker_data/pgsql/data/init_dump.sql | docker exec -i $(env)-$(project)--postgres psql -U project-ex project-ex_test

bash-front:
  @make dc cmd="exec $(env)-$(project)--front sh" env="$(env)"

...
```

Make создает своего рода короткий алиас команд для управления сервисами. В нём можно инициализировать проект, пересоздать базу, собрать фронт и так далее. Эти же команды мы будем использовать в GitLab CI файле.

Далее мы запускаем команду `make init` для инициализации проекта.

```
yaml
version: '3'
services:
 stage-project-ex--traefik:
   image: "traefik:v3.0.0-beta2"
   container_name: "stage-project-ex--traefik"
   command:
     - "--log.level=DEBUG"
     - "--providers.docker=true"
     - "--providers.docker.exposedbydefault=false"
     - "--entrypoints.web.address=:80"
     - "--entrypoints.web.http.redirections.entryPoint.to=websecure"
     - "--entrypoints.websecure.address=:443"
     - "--entrypoints.postgres.address=:5432"
     - "--entrypoints.clickhouse.address=:8123"
     - "--entrypoints.mongo.address=:27017"
     - "--certificatesresolvers.stage_project-ex_app.acme.httpchallenge=true"
     - "--certificatesresolvers.stage_project-ex_app.acme.httpchallenge.entrypoint=web"
     - "--certificatesresolvers.stage_project-ex_app.acme.email=it@email-ex.com"
     - "--certificatesresolvers.stage_project-ex_app.acme.storage=/letsencrypt/acme.json"
   restart: always
   ports:
     - 80:80
     - 443:443
     - 5432:5432
     - 8123:8123
     - 27017:27017
   networks:
     - stage_project-ex_network
   volumes:
     - "/opt/letsencrypt:/letsencrypt"
     - "/var/run/docker.sock:/var/run/docker.sock:ro"

networks:
 stage_project-ex_network:
   external: true
   name: stage_project-ex_network
```

### Настройка портов

В compose-файле ничего не сказано про порты. Основная причина в том, что обращаться к проекту мы будем по доменному имени с помощью [Traefik](https://hub.docker.com/_/traefik). Приложения работает отдельно от compose-файлов и версий проекта: о появлении новых контейнеров Traefik узнаёт от Docker Daemon, а конфигурация для приложения прописывается в compose-файле после ключевого слова .

Traefik проксирует трафик к контейнеру на основе hostname (не только по HTTP/HTTPS), запрашивает LE-сертификат, сам его продлевает. При это указывать, на какой IP или hostname проксировать или менять конфиг Traefik не нужно.

> Если мы поднимаем локальные контейнеры с локальным доменным именем, запросить LE-сертификат не получится. Поэтому с web придётся общаться по HTTP, а в Traefik отключить редирект на HTTPS

Версия образа traefik:v3.0.0-beta2 выбрана не случайно, она поддерживает различные доменные имена для маршрутизации к контейнерам PostgreSQL. В примере выше использование beta2 не обязательно, так как любой запрос на порт 5432 будет проксироваться на единственный контейнер с PostgreSQL.

### Когда postgres контейнеров несколько

Для работы с несколькими PostgreSQL-контейнерами и маршрутизации к ним на основе доменных имен требуется создать самоподписанный Wildcard-сертификат локального домена и добавить информацию о нем в конфиг Traefik.

> Это делается только для того, чтобы к PostgreSQL-контейнерам можно было обращаться «извне» для работы с базой напрямую. В случае, если контейнеры работают в одной Docker-сети, Traefik не нужен.

В compose traefik добавляем:

```
yaml
    command:
      - "--providers.file.filename=/conf/dynamic-conf.yml"
    volumes:
      - "./tls:/tls"
      - "./conf:/conf"
```

В conf/dynamic-conf.yml прописываем файлы сертификатов

```
ymal
tls:
  certificates:
    - certFile: /tls/something.com.pem
      keyFile: /tls/something.com.key
```

В директорию tls/ кладём файлы Wildcard-сертификата, созданные Bash-скриптом `mkcert.sh`

```
bash
#!/usr/bin/env bash
set -euo pipefail
IFS=$'\n\t'

 DOMAIN_NAME=$1

if [ ! -f $1.key ]; then

  if [ -n "$1" ]; then
    echo "You supplied domain $1"
    SAN_LIST="[SAN]\nsubjectAltName=DNS:localhost, DNS:*.localhost, DNS:*.$DOMAIN_NAME, DNS:$DOMAIN_NAME"
    printf $SAN_LIST
  else
    echo "No additional domains will be added to cert"
    SAN_LIST="[SAN]\nsubjectAltName=DNS:localhost, DNS:*.localhost"
    printf $SAN_LIST
  fi

  openssl req \
    -newkey rsa:2048 \
    -x509 \
    -nodes \
    -keyout "$1.key" \
    -new \
    -out "$1.crt" \
    -subj "/CN=compose-dev-tls Self-Signed" \
    -reqexts SAN \
    -extensions SAN \
    -config <(cat /etc/ssl/openssl.cnf <(printf $SAN_LIST)) \
    -sha256 \
    -days 3650

  echo "new TLS self-signed certificate created"

else

  echo "certificate files already exist. Skipping"

fi
```

Правим `labels` контейнера postgres в compose-файле

```
yaml
    labels:
      - "traefik.enable=true"
      - "traefik.tcp.routers.qa222_postgres.rule=HostSNI(`qa222.something.com`)"
      - "traefik.tcp.routers.qa222_postgres.entryPoints=postgres"
      - "traefik.tcp.routers.qa222_postgres.service=qa222_postgres"
      - "traefik.tcp.services.qa222_postgres.loadbalancer.server.port=5432"
      - "traefik.tcp.routers.qa222_postgres.tls=true"
```

### CI/CD проекта

Ниже приложили наш GitLab CI файл, в нем мы можем видеть ранее озвученные `make`-команды

```
yaml
variables:
 APP4_ENV: "gitlab"

default:
 tags:
   #gtilab runner tag
   - dev-project-ex-1

stages:
 - ci
 - delivery
 - build
 - deploy

.before_script_template: &build_test-integration
 before_script:
 - echo "Prepare job"
 - sed -i "s!env=local!env=${APP4_ENV}!" ./Makefile
 - make cp-env
 - make cp-yml
 - make up

.verify-code: &config_template
 stage: ci
 <<: *build_test-integration
 only:
   refs:
     - merge_requests
     - develop
     - master

Linter:
 <<: *config_template
 script:
   - make build
   - make linter

Tests:
 <<: *config_template
 script:
   - make tests

Delivery:
 stage: delivery
 script:
   - echo "Rsync from $CI_PROJECT_DIR"
   - sudo rm -rf "/home/project-ex/stands/dev/project-ex/!\(static|node_modules\)"
   - sed -i "s!env=local!env=dev!" ./Makefile
   - rsync -av --delete-before --no-perms --no-owner --no-group
     --exclude "node_modules/"
     --exclude "__pycache__/"
     --exclude "logs/"
     --exclude "docker-compose/docker_data/clickhouse/data/"
     $CI_PROJECT_DIR/ /home/project-ex/stands/dev/project-ex
 only:
   - develop
 except:
   - master

Build:
 stage: build
 script:
   - echo "cd /home/project-ex/stands/dev/project-ex"
   - cd /home/project-ex/stands/dev/project-ex
   - echo "make cp-env"
   - make cp-env
   - echo "cp-yml"
   - make cp-yml
   - echo "build"
   - make build
 only:
   - develop
 except:
   - master

Build-front:
 stage: build
 script:
   - echo "cd /home/project-ex/stands/dev/project-ex"
   - cd /home/project-ex/stands/dev/project-ex
   - echo "build-front"
   - make build-front
 only:
   changes:
     - '*.js'
     - '*.css'
     - '*.less'
   refs:
     - develop
     - master

Deploy:
 stage: deploy
 script:
   - cd /home/project-ex/stands/dev/project-ex
   - mkdir -p logs
   - make restart
   - make migrate
   - make collect-static
 only:
   - develop
 except:
   - master
```

## Плюсы и минусы Docker

Docker использует ядро операционной системы хоста. Это вводит ряд ограничений: инструмент работает только на 64-битной установке Linux с ядром версии 3.10 или старше. Также он рассчитан на серверные приложения, и работу с графическими интерфейсами не всегда поддерживает.

Вдовесок, Docker требует от разработчика четкости и аккуратности. Неправильная конфигурация контейнера или недостаточные меры безопасности могут поставить всю систему под угрозу.

Как понимаете, у инструмента тоже есть свои недостатки. Но преимуществ несомненно больше: Docker использует общие ядра операционных систем и изолирует приложения с использованием namespaces и cgroups — нет необходимости под каждую задачу запускать отдельную виртуальную машину. Также он оптимизирует распределение ресурсов по контейнерам, способен управлять жизненным циклом приложения — запускать, останавливать, масштабировать и обновлять контейнеры. И самый большой плюс — экосистема и живое комьюнити. В Docker Hub лежит сотни готовых образов, а в сообществе можно задать вопрос или найти готовое решение.

Главное, помните: монолитная архитектура не означает «плохой», «устаревший» или «не модный» подход. Проектировать приложение стоит исходя из здравого смысла и оценки в том числе собственных возможностей. Потому что внедрение микросервисов с CI/CD требует больше навыков и компетенций, чем автоматизация монолитного приложения.

