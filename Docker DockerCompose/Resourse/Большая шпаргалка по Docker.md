



# Большая шпаргалка по Docker: как распилить монолитный проект на части


## Туториал

Погружение в мир контейнеризации с докером — это путь к оптимизации развёртыванию приложений, а также ключ к упрощению жизни разработчиков и системных администраторов. Меня зовут Андрей Аверков, в IT c 2008 начинал пусть с аналитика-проектировщика IT систем, 11 лет в роли разработчика и последние годы на руководящих должностях. Сейчас я тимлид команды разработки из 9 человек в группе компаний Kokoc Group. Мы занимаемся созданием и поддержкой CPA платформ (gdeslon.ru, fxpartners.ru, ads.mobisharks.com), а также проектом по генерации лендингов - lpgenerator.ru. Отдел разработки тесно сотрудничает с отделом IT. И эту статью мне помогал писать [@Egorov_Ilja](https://habr.com/users/egorov_ilja), он руководит IT-отделом в группе компаний Kokoc Group с 2013 года, а вообще в компании уже более 16 лет. В общем, у нас довольно большой опыт в разделении продуктов на части, поэтому, сегодня мы собрали самое основное и необходимое для работы с Docker. В нашей шпаргалке вы найдете все необходимое для успешного старта с докером: от базовых концепций и установки до продвинутых техник работы с контейнерами.

## Что такое контейнеры и Docker

Перед тем, как рассказывать о Docker, погрузимся немного в теорию, что такое контейнеры и зачем они нужны. Если вы в курсе, можете переходить сразу ко второй части (чуть ниже), тут будет только база. И сразу простите, тут мало картинок и много кода.

**Контейнеры** представляют собой метод стандартизации развертывания приложений и их отделения от общей инфраструктуры. Экземпляр приложения запускается в изолированной среде, не влияющей на основную операционную систему.

Для разработчиков это означает, что им не нужно беспокоиться о том, в каком окружении будет функционировать их приложение, наличии необходимых настроек и зависимостей. Они могут просто создать приложение, упаковать все зависимости и настройки в единый образ. Этот образ легко запускать на различных системах, без опасений, что приложение не сможет стартовать.

**Docker** является платформой для разработки, доставки и запуска контейнерных приложений. Docker позволяет создавать контейнеры, автоматизировать их запуск и развертывание, а также управлять их жизненным циклом. Эта платформа обеспечивает возможность запуска множества контейнеров на одной хост-машине, что обеспечивает высокую степень гибкости и масштабируемости при разработке, тестировании и внедрении приложений.

**Docker Daemon** представляет собой сервер, функционирующий в фоновом режиме. Он прослушивает запросы от Docker CLI и управляет жизненным циклом контейнеров. Этот сервер отвечает за выполнение различных задач, включая запуск, остановку контейнеров, а также регулирование сетей и портов, а также ведение логов контейнеров.

**Docker CLI** представляет интерфейс командной строки Docker. Через команды, введенные в командной строке, пользователь указывает Docker Daemon выполнить конкретные действия, такие как поднятие, запуск или остановка контейнеров.

Важно отметить, что Docker CLI можно устанавливать как на локальной системе, так и настраивать для удаленного доступа, взаимодействуя с Docker Daemon посредством **REST API**. Этот механизм обеспечивает гибкость в управлении Docker из различных окружений.

Функциональность Docker Daemon расширяется за пределы простого управления жизненным циклом контейнеров. Эта подсистема также регулирует сетевые настройки, порты и ведение логов контейнеров. Ниже перечислены наиболее часто используемые команды для взаимодействия с Docker Daemon, которые включают в себя различные аспекты управления контейнерами и системными ресурсами.

## Шпаргалка по Docker CLI

**Основные команды**

- docker run <имя_образа> # запуск контейнера на основе указанного образа
 - docker ps # показать список активных контейнеров
- docker ps -a # показать все контейнеры, включая остановленные
- docker stop <идентификатор_контейнера> # остановить контейнер
- docker rm <идентификатор_контейнера> # удалить контейнер
- docker images # показать список всех локальных образов
- docker pull <имя_образа> # загрузить образ из Docker Hub
- docker rmi <идентификатор_образа> # удалить локальный образ

**Создание и работа с образами**

docker build -t <имя_образа>:<тег> <путь_к_Dockerfile> # сборка образа на основе Dockerfile
docker tag <старый_тег> <новый_тег> # пометить образ новым тегом
docker tag <имя_образа>:<старый_тег> <новый_репозиторий>/<новый_тег> # переименовать и пометить образ для загрузки в другой репозиторий
docker push <имя_репозитория>/<имя_образа>:<тег> # отправка образа в Docker Hub или другой реестр



**Сети и порты**

```
docker network ls # показать список сетейdocker run -p <локальный_порт>:<контейнерный_порт> <имя_образа> # определить соответствие портов при запуске контейнера
```

**Работа с Docker Compose**

```
docker-compose up # запустить сервисы, определенные в файле docker-compose.ymldocker-compose downмостановить и удалить сервисы, описанные в файле docker-compose.yml
```

**Работа с Docker Volumes**

```
docker volume create <имя_volume> # создать Docker Volumedocker run -v <имя_volume>:<путь_в_контейнере> <имя_образа> # запустить контейнер, подключив Volume
```

**Логирование и Мониторинг**

```
docker logs <идентификатор_контейнера> # показать логи контейнераdocker stats <идентификатор_контейнера> # отобразить статистику использования ресурсов контейнера
```

## Dockerfile

Команды по созданию образа фиксируются в необработанном (raw) текстовом документе — Docker-файле:

```
ИНСТРУКЦИЯ аргумент(s)
```

где <ИНСТРУКЦИЯ> — команда для Docker Daemon, а <аргумент> — сам аргумент или конкретные значения, которые передаются в <ИНСТРУКЦИЮ>.

**_NB_**_: инструкции нечувствительны регистру, однако их принято писать «капсом», чтобы визуально отличать от аргументов._

Инструкции объясняют, что Docker Daemon должен сделать до, во время или после запуска контейнера из образа.

Основные инструкции для Dockerfile

Пример Docker-файла для приложения на Python:

```
# Используем базовый образ с PythonFROM python:3.8# Устанавливаем зависимостиRUN pip install flask# Копируем исходный код в образCOPY . /app# Указываем рабочую директориюWORKDIR /app# Определяем команду для запуска приложенияCMD ["python", "app.py"]
```

## Образ Docker (Docker Image)

Для того, чтобы из Dockerfile создать образ и запустить контейнер, нужно:: 

1. Перейти в директорию, где лежит dockerfile.
    
2. Командой docker build создать образ из файла.
    
3. По необходимости, проверить образы командой docker images.
    
4. Запустить контейнер из образа командой docker run.
    

В работе с образами можно использовать теги, чтобы указывать версию образов. По умолчанию, Docker присваивает тег _<latest>_ при сборке.

```
#пример сборки образа с явным указанием тегаdocker build -t my-python-app:v1.0
```

Для отправки образа в реестр Docker Hub используются следующие команды:

```
docker tag my-python-app:v1.0 username/my-python-app:v1.0docker push username/my-python-app:v1.0
```

Загрузка образа из реестра выполняется командой:

```
docker pull username/my-python-app:v1.0
```

Образы Docker статичны. А вот контейнеры  — изменяемы. Чтобы «обновить» образ, можно запустить из него контейнер, внести изменения и сохранить состояние в новый образ. Делается это с использованием команды _<docker commit>_:

```
docker commit -m "Добавлены изменения" -a "Автор" container_id username/my-python-app:v1.1
```

Образ Docker представляет собой стандартный формат, который обеспечивает совместимость с Docker Daemon на любой платформе. Эта характеристика позволяет переносить проекты между разными системами без затруднений — контейнеры упаковываются в образы и легко перемещаются. Изоляция всех зависимостей и компонентов внутри образа обеспечивает уверенность в том, что проект успешно "встанет" на целевую платформу с Docker без необходимости дополнительной настройки.

Так, стандартизированный формат образа в сочетании с изолированной средой выполнения контейнера делает процесс переноса проектов между различными системами беспроблемным и эффективным.

## Контейнер (Docker Container)

Мы уже вкратце рассказали в начале про контейнеры, но давайте закрепим и перейдём к сути. Контейнер — это работающий в изолированной среде экземпляр образа (инстанс). В один контейнер «упаковывается» один работающий серверный процесс. 

Возможно, конечно, вместить несколько процессов в один контейнер, даже создать монолитную структуру — Docker не налагает жестких ограничений в этом плане. Однако, следует отметить, что такой подход рассматривается как ошибка в проектировании микросервисной архитектуры. Docker предоставляет средства для настройки взаимодействия контейнеров с внешней средой и другими контейнерами, а также управление потреблением ресурсов. Поэтому нет веских причин стараться уместить все компоненты в одном контейнере.

Дополнительные возможности Docker включают в себя способность настраивать взаимодействие между контейнерами и их внешней средой, а также регулирование потребления ресурсов. Если необходимо, чтобы контейнер работал с собственным экземпляром данных, не затрагивая оригинал, можно осуществить монтирование директории из хост-системы в сам контейнер. Этот процесс выполняется с использованием соответствующей команды Docker, что дает дополнительный уровень гибкости при работе с данными внутри контейнера.

```
docker run -v /путь/к/хост-директории:/путь/в/контейнере имя_образа
```

Docker Volumes — хранилища, которые связываются с контейнером, но не привязываются к его жизненному циклу. Это означает, что любые данные, которые контейнер отправляет в Volumes, сохранятся даже если контейнер остановить или уничтожить.

```
#команда для создания Volume в контейнереdocker run -v my_volume:/путь/в/контейнере имя_образа
```

## Реестр Docker

Реестр Docker, или Docker Registry, представляет собой общедоступный репозиторий образов, который выполняет несколько важных функций. Этот сервис позволяет:

- **Централизованное хранение образов и их версий:** Docker Registry предоставляет единое место для хранения образов контейнеров и их различных версий, обеспечивая удобный доступ к ним.
    
- **Ускорение развертывания:** Образы скачиваются с реестра непосредственно на целевую систему, что ускоряет процесс развертывания и готовит контейнеры к оперативной работе.
    
- **Автоматизация процессов сборки, тестирования и развертывания контейнеров:** Docker Registry интегрируется в процессы разработки, позволяя автоматизировать сборку, тестирование и развертывание контейнеров.
    

Docker Hub является публичным реестром, где хранятся общедоступные образы, такие как дистрибутивы Linux, базы данных, языки программирования и многое другое. Важно отметить, что организации могут создавать собственные приватные реестры Docker для хранения конфиденциальных данных, обеспечивая дополнительный уровень безопасности и управления доступом к образам контейнеров.

Чтобы передать переменные окружения в контейнер, используется флаг <-e> совместно с командой <RUN>:

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

#### Создание приватного реестра Docker

### Установка Docker Distribution

Docker Distribution  — это официальная реализация протокола Docker Registry. Установим его на сервер, который будет служить приватным реестром.

```
docker run -d -p 5000:5000 --restart=always --name registry registry:2
```

Эта команда запускает приватный реестр на порту 5000. Опционально можно настроить HTTPS с использованием SSL-сертификата.

### Использование Приватного Реестра

Теперь можно использовать реестр для хранения и распространения приватных Docker-образов.

```
# помечаем образ   docker tag my-image localhost:5000/my-image# отправляем образ в приватный реестрdocker push localhost:5000/my-image
```

## Запускаем монолит на сервере без Docker

В README приложения можно найти подробную инструкцию как развернуть его на сервере. Для примера возьмём README монолитного приложения, который мы намеренно сократили:

**Установка пакетов:**

```
sudo apt-get install -y build-essential libssl-dev zlib1g-dev libbz2-dev \       libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \       xz-utils tk-dev libffi-dev liblzma-dev python-openssl git npm redis-server vim ffmpeg
```

**Установка pyenv:**

```
$ curl https://pyenv.run | bash$ echo 'export PATH="$HOME/.pyenv/bin:$PATH"' >> ~/.bashrc && \echo 'eval "$(pyenv init -)"' >> ~/.bashrc && \echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc && source ~/.bashrc
```

**Установки Python 3.6.9:**

```
pyenv install 3.6.9
```

Если установлена версия Ubuntu 20+ и возникла ошибка при попытке установки, применить следующие команды:

```
$ sudo apt install clang -y$ CC=clang pyenv install 3.6.9
```

**Создание виртуального окружения:**

```
$ pyenv virtualenv 3.6.9 cpa-project
```

**Активация виртуального окружения:**

```
$ pyenv activate cpa-project
```

**Установка NodeJS 8.11.3:**

```
$ npm i n -g$ sudo n install 8.11.3$ sudo n # в появившемся окне выбираем версию 8.11.3
```

**Клонирование проекта**

```
   $ git clone git@github.com:User/cpa-project.git
```

**Переходим в проект:**

```
$ cd cpa-project
```

**Установка зависимостей python (убедитесь, что виртуальное окружение активно):**

```
$ pip install -U pip$ pip install -r requirements.txt
```

**Установка зависимостей NodeJS:**

```
$ npm install
```

**Проводим миграции для базы данных и создаем тестовые данные:**

```
$ python manage.py migrate$ python manage.py generate_test_data
```

#### Сборка клиентской части

```
$ npm run watch # Для разработки с автоматическим ребилдом$ npm run build # Для продакшена
```

Как видите, для запуска монолита на сервере прописываются и выполняются множества команд, а процесс требует участия человека.

## Собираем тот же проект в Docker

Давайте посмотрим, как это же приложение можно развернуть в среде Docker. Для начала выделим сервисы для compose-файла и придумаем им названия:

Compose-файл

```

```

**Docker Compose** — инструмент для запуска многоконтейнерных приложений в Docker. В .yaml-файле указываются все необходимые настройки и команды. Запуск контейнеров из compose-файла производится командой <docker-compose up>.

В .yaml-файле мы можем увидеть из каких контейнеров _<container_name>_ (и каких версий) запускается наше ранее монолитное приложение. А ключевое слово _{stage}_  — это ветка в GitLab, из которой будет подниматься контейнер. По желанию, на одном сервере мы можем запускать контейнеры из разных веток.

_От того, что мы разбили приложение на микросервисы, оно не перестало быть монолитным. Микросервисность продукта закладывается на стадии его проектирования и создания, когда каждая задача выделяется в отдельный сервис._

Строка в compose <dockerfile: Dockerfile> собирает наш контейнер. В файле содержатся такие инструкции:

```
dockerfileFROM python:3.6.9-busterENV DJANGO_SETTINGS=advgame.local_settingsARG DEBIAN_FRONTEND=noninteractiveRUN apt-get update \ # dependencies for building Python packages && apt-get install -y build-essential \ # psycopg2 dependencies && apt-get install -y libpq-dev \ # Translations dependencies && apt-get install -y gettext \ # Cron && apt-get install -y cron \ # Vim && apt-get install -y vim \ # cleaning up unused files && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \ && rm -rf /var/lib/apt/lists/*# Set timezoneENV TZ=Europe/MoscowRUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone# Have to invalidate cache here because Docker is bugged and doesn't invalidate cache# even if requirements.txt did changeADD ../requirements.txt /requirements.txtRUN pip install -r /requirements.txtCOPY ./docker-compose/start.sh /startRUN chmod +x /start# Copy hello-cron file to the cron.d directoryCOPY ./docker-compose/crontab.docker /etc/cron.d/crontab.docker# Give execution rights on the cron jobRUN chmod 0644 /etc/cron.d/crontab.docker# Apply cron jobRUN crontab /etc/cron.d/crontab.dockerCOPY . /appWORKDIR /app
```

Далее мы написали <make>-файл который позволит нам управлять конфигурированием проекта:

```
makedir=${CURDIR}project=project-exenv=localinteractive:=$(shell [ -t 0 ] && echo 1)ifneq ($(interactive),1)  optionT=-Tendifuid=$(shell id -u)gid=$(shell id -g)# Команда для docker-compose execc=# Параметр для docker-compose execp=dc:  @docker-compose -f ./docker-compose/$(env).yml --env-file=./docker-compose/.env.$(env) $(cmd)compose-logs:  @make dc cmd="logs" env="$(env)"cp-env:  [ -f ./docker-compose/.env.$(env) ] && echo ".env.$(env) file exists" || cp ./docker-compose/.env.example ./docker-compose/.env.$(env)  sed -i "s/{stage}/$(env)/g" ./docker-compose/.env.$(env)  @if [ "$(env)" = "local" ] ; then \     sed -i "s/{domain}/ma.local/g" ./docker-compose/.env.$(env) ; \  fi;  @if [ "$(env)" = "dev" ] ; then \     sed -i "s/{domain}/dev.project-ex.io/g" ./docker-compose/.env.$(env) ; \  fi;cp-yml:  @if [ ! "$(env)" = "local" ] ; then \     [ -f ./docker-compose/$(env).yml ] && echo "$(env).yml file exists" || cp ./docker-compose/stage.example.yml ./docker-compose/$(env).yml ; \     sed -i "s/{stage}/$(env)/g" ./docker-compose/$(env).yml; \  fi;init:  docker network ls | grep stage_project-ex_network > /dev/null || docker network create stage_project-ex_network  @make cp-env  @make cp-yml  [ -f ./docker-compose/.env.$(env) ] && echo ".env.$(env) file exists" || cp ./docker-compose/.env.$(env).example ./docker-compose/.env.$(env)  @make dc cmd="up -d"  @make dc cmd="start $(env)-$(project)--postgres" env="$(env)"  sleep 5 && cat ./docker-compose/docker_data/pgsql/data/init_dump.sql | docker exec -i $(env)-$(project)--postgres psql -U project-ex  @make dc cmd="exec $(env)-$(project)--app python ./manage.py migrate" env="$(env)"  @make ch-restore env="$(env)"  @make build-front env="$(env)"  @make collect-static env="$(env)"create_test_db:  @make dc cmd="exec $(env)-$(project)--postgres dropdb --if-exists -U project-ex project-ex_test" env="$(env)" > /dev/null  @make dc cmd="exec $(env)-$(project)--postgres createdb -U project-ex project-ex_test" env="$(env)"  cat ./docker-compose/docker_data/pgsql/data/init_dump.sql | docker exec -i $(env)-$(project)--postgres psql -U project-ex project-ex_testbash-front:  @make dc cmd="exec $(env)-$(project)--front sh" env="$(env)"
```

<make> создает своего рода короткий алиас команд для управления сервисами. В нём можно инициализировать проект, пересоздать базу, собрать фронт и так далее. Эти же команды мы будем использовать в GitLab CI файле. Далее мы запускаем команду _<make init>_ для инициализации проекта.

---

Переходим к другому compose файлу вспомогательного сервиса. Данному сервису не требуется частая перезагрузка, его достаточно настроить «один» раз, а далее он сам следит за всеми контейнерами Docker-демона.

Compose-файл

```

```

### Настройка портов

В первом compose-файле ничего не сказано про порты. Основная причина в том, что обращаться к проекту мы будем по доменному имени с помощью [Traefik](https://hub.docker.com/_/traefik). Приложения работает отдельно от compose-файлов и версий проекта: о появлении новых контейнеров Traefik узнаёт от Docker Daemon, а конфигурация для приложения прописывается в compose-файле после ключевого слова _<labels>_.

Traefik проксирует трафик к контейнеру на основе hostname (не только по HTTP/HTTPS), запрашивает LE-сертификат, сам его продлевает. При этом указывать, на какой IP или hostname проксировать или менять конфиг Traefik не нужно.

_Если мы поднимаем локальные контейнеры с локальным доменным именем, запросить LE-сертификат не получится. Поэтому с web придётся общаться по HTTP, а в Traefik отключить редирект на HTTPS_

Версия образа traefik:v3.0.0-beta2 выбрана не случайно, она поддерживает различные доменные имена для маршрутизации к контейнерам PostgreSQL. В примере выше использование beta2 не обязательно, так как любой запрос на порт 5432 будет проксироваться на единственный контейнер с PostgreSQL.

### Когда postgres контейнеров несколько

Для обеспечения работы с несколькими контейнерами PostgreSQL и настройки маршрутизации по доменным именам требуется генерация самоподписанного Wildcard-сертификата для локального домена и интеграция информации о нем в конфигурацию Traefik.

Этот процесс необходим исключительно для обеспечения внешнего доступа к контейнерам PostgreSQL напрямую. При использовании Docker-сети, в которой контейнеры находятся, использование Traefik становится излишним.

В <compose traefik> добавляем:

```
yaml    command:      - "--providers.file.filename=/conf/dynamic-conf.yml"    volumes:      - "./tls:/tls"      - "./conf:/conf"
```

В conf/dynamic-conf.yml прописываем файлы сертификатов:

```
yamltls:  certificates:    - certFile: /tls/something.com.pem      keyFile: /tls/something.com.key
```

В директорию tls/ кладём файлы Wildcard-сертификата, созданные Bash-скриптом  <[mkcert.sh](http://mkcert.sh/) [something.com](http://something.com/)>:

```
bash#!/usr/bin/env bashset -euo pipefailIFS=$'\n\t' DOMAIN_NAME=$1if [ ! -f $1.key ]; then  if [ -n "$1" ]; then    echo "You supplied domain $1"    SAN_LIST="[SAN]\nsubjectAltName=DNS:localhost, DNS:*.localhost, DNS:*.$DOMAIN_NAME, DNS:$DOMAIN_NAME"    printf $SAN_LIST  else    echo "No additional domains will be added to cert"    SAN_LIST="[SAN]\nsubjectAltName=DNS:localhost, DNS:*.localhost"    printf $SAN_LIST  fi  openssl req \    -newkey rsa:2048 \    -x509 \    -nodes \    -keyout "$1.key" \    -new \    -out "$1.crt" \    -subj "/CN=compose-dev-tls Self-Signed" \    -reqexts SAN \    -extensions SAN \    -config <(cat /etc/ssl/openssl.cnf <(printf $SAN_LIST)) \    -sha256 \    -days 3650  echo "new TLS self-signed certificate created"else  echo "certificate files already exist. Skipping"fi
```

Правим <labels> контейнера postgres в compose-файле:

```
yaml    labels:      - "traefik.enable=true"      - "traefik.tcp.routers.qa222_postgres.rule=HostSNI(`qa222.something.com`)"      - "traefik.tcp.routers.qa222_postgres.entryPoints=postgres"      - "traefik.tcp.routers.qa222_postgres.service=qa222_postgres"      - "traefik.tcp.services.qa222_postgres.loadbalancer.server.port=5432"      - "traefik.tcp.routers.qa222_postgres.tls=true"
```

### CI/CD проекта

![](https://habrastorage.org/r/w1560/getpro/habr/upload_files/6e6/098/ecc/6e6098ecc0ff5542f8506d69d6d328eb.jpg)

Ниже приложили наш GitLab CI файл, в нем мы можем видеть ранее озвученные <make>-команды. Для того, чтобы развернуть проект на сервере, был настроен ci/cd, который позволяет:

- проверить качество кода;
    
- запустить тесты;
    
- собрать билды образов докера;
    
- доставить всё это до сервера.
    

Данный ci использует алиасы make файла, про который мы писали выше.

```
yamlvariables: APP4_ENV: "gitlab"default: tags:   #gtilab runner tag   - dev-project-ex-1stages: - ci - delivery - build - deploy.before_script_template: &build_test-integration before_script: - echo "Prepare job" - sed -i "s!env=local!env=${APP4_ENV}!" ./Makefile - make cp-env - make cp-yml - make up.verify-code: &config_template stage: ci <<: *build_test-integration only:   refs:     - merge_requests     - develop     - masterLinter: <<: *config_template script:   - make build   - make linterTests: <<: *config_template script:   - make testsDelivery: stage: delivery script:   - echo "Rsync from $CI_PROJECT_DIR"   - sudo rm -rf "/home/project-ex/stands/dev/project-ex/!\(static|node_modules\)"   - sed -i "s!env=local!env=dev!" ./Makefile   - rsync -av --delete-before --no-perms --no-owner --no-group     --exclude "node_modules/"     --exclude "__pycache__/"     --exclude "logs/"     --exclude "docker-compose/docker_data/clickhouse/data/"     $CI_PROJECT_DIR/ /home/project-ex/stands/dev/project-ex only:   - develop except:   - masterBuild: stage: build script:   - echo "cd /home/project-ex/stands/dev/project-ex"   - cd /home/project-ex/stands/dev/project-ex   - echo "make cp-env"   - make cp-env   - echo "cp-yml"   - make cp-yml   - echo "build"   - make build only:   - develop except:   - masterBuild-front: stage: build script:   - echo "cd /home/project-ex/stands/dev/project-ex"   - cd /home/project-ex/stands/dev/project-ex   - echo "build-front"   - make build-front only:   changes:     - '*.js'     - '*.css'     - '*.less'   refs:     - develop     - masterDeploy: stage: deploy script:   - cd /home/project-ex/stands/dev/project-ex   - mkdir -p logs   - make restart   - make migrate   - make collect-static only:   - develop except:   - master
```

## Плюсы и минусы Docker

Docker использует ядро операционной системы хоста, что накладывает определенные ограничения на его использование. Инструмент функционирует только на 64-битной установке Linux с ядром версии 3.10 или более поздней. Кроме того, он ориентирован на серверные приложения и не всегда поддерживает работу с графическими интерфейсами. Важно отметить, что неправильная конфигурация контейнера или недостаточные меры безопасности могут представлять угрозу для всей системы.

Однако, несмотря на эти ограничения, преимущества Docker ощутимо перевешивают. Использование общих ядер операционных систем и изоляция приложений через namespaces и cgroups устраняет необходимость в запуске отдельных виртуальных машин для каждой задачи. Docker также оптимизирует распределение ресурсов между контейнерами и обеспечивает управление жизненным циклом приложения, включая запуск, остановку, масштабирование и обновление контейнеров. Кроме того, существует развитая экосистема Docker Hub, где хранятся сотни готовых образов, и активное сообщество, где можно обсудить вопросы или найти решения.

Важно помнить, что, несмотря на преимущества микросервисной архитектуры, монолитный подход также имеет свое место и может быть целесообразным, особенно в зависимости от требований проекта и компетенций команды разработчиков.

