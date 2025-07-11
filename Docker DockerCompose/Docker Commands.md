docker version
docker -v


`# качает образ с docker hub и сохраняет локально`
docker pull hello-world


`# ищет образ локально, если его нет, то ищет и качает его с docker hub,`
`# и запускает контейнер`
docker run hello-world


`# посмотреть все запущенные и остановленные контейнеры (all), без -а только`
`# запущенные`
docker ps -a


`# список всех локальных образов`
docker images


`# запустить остановленный контейнер`
docker start <id or name>


`# запустить остановленный контейнер с привязкой вывода`
docker start -i <id or name>


`# указать имя (app например) для контейнера при запуске`
docker run --name hello hello-world


`# остановить контейнер`
docker stop <id or name>


`# удалить остановленный контейнер`
docker rm <id or name>

`# вывести id контейнеров`
docker ps -qa

`# удалить все остановленные контейнеры`
docker container prune
docker rm $(docker ps -qa)


`# запустить контейнер и удалить его после остановки`
docker run --rm image:tag


`# удалить image`
docker rmi <id or image:tag>


`# ищет образ локально, если его нет, то ищет и качает его с docker hub,`
`# и запускает контейнер`
docker run ubuntu (контейнер сразу остановится, т.к. нет запущеных процессов)


`# запуск с процессом интерактивного (-i) терминала (-t)`
docker run -i -t ubuntu
docker run -it ubuntu


`# создание образа из докерфайла (-t флаг для указания имени и тега образа)`
docker build . -t app:1.0.0
docker build -t app:1.0.0 .


`# создание новой версии образа из докерфайла`
docker build . -t app:2.0.0


`# Запуск в сессии терминала`
docker run ngnix


`# Запуск с демоном отвязанным от сессии терминала`
docker run -d nginx (detached - открепленный)


`# Просмотр логов контейнера`
docker logs <id or name>


`# Просмотр последних логов контейнера (не всех вообщем)`
docker logs <id or name> | tail (для windows PowerShell | Select-Object -Last 10)


`# Логи в файл`
docker logs <id_or_name> > logs.txt


`# Просмотр логов контейнера в режиме трансляции и в терминале можно что-то печатать чтоб делать метки`
docker logs <id or name> -f


`# просмотр подробной информации о контейнере`
docker container inspect <id or name>
docker container inspect <id or name> | grep (findstr for windiws) IPAddress
docker container inspect <id or name> | grep (findstr for windiws) Mounts


`# чтоб создать новый процесс в УЖЕ запущенном контейнере с процессом bash`
docker exec -it <id or name> <bash or sh>


`# Через exec вы можете выполнять любые команды в запущенном контейнере`
docker exec -it mydbcontainer psql -U postgres


`# Монтирование портов`
docker run -p 8000:80 -d --name server nginx


`# Монтирование портов и ip`
docker run -p 127.0.0.1:8000:80 -d --name server nginx


`# Монтирование директорий`
docker run -p 8000:80 -d -v ${PWD}/site:/usr/share/nginx/html --name server nginx


`# Создать контейнер (сеть контейнеров) из файла compose.yml`
docker compose up
docker compose up --build ('с пересборкой если есть изменения')

`# Создать контейнер (сеть контейнеров) из файла compose.yml с открепленным процессом`
docker compose up -d
docker compose up -d --build ('с пересборкой если есть изменения')


`# Остановить и удалить сеть контейнеров (контейнер)`
docker compose down


`# Останавливаем все контейнеры, не удаляя их`
docker compose stop


`# Запускаем все ранее остановленные контейнеры`
docker compose start


`# Создание сети`
docker network create my_network


`# Запуск контейнера монго и подключение его к сети`
docker run -d --name mymongo --network my_network mongo


`# Запуск контейнера Mongo express, подключение его к сети, задание переменной`
`# окружения с параметрами доступа и указание порта`
docker run -d --name mymongo-express --network my_network -e ME_CONFIG_MONGODB_SERVER=mymongo -e ME_CONFIG_BASICAUTH_USERNAME=root -e ME_CONFIG_BASICAUTH_PASSWORD=root -p 8081:8081 mongo-express


`# Запуск контейнера с приложением`
docker run -d --name myapp --network my_network app


`# Просмотреть логи контейнера`
docker logs <id or name>
docker compose logs -f


`# Перезапуск всех контейнеров или одного`
docker compose restart
docker compose restart adminer


`# Режим синхронизации кода во время разработки`
docker compose up --watch


`# проверка наличия ошибок в журнале`
docker compose logs -f


`# просмотр логов сети контейнеров`
docker compose -f docker-compose.prod.yml logs -f


`# Остановить и удалить контейнеры и примонтированные тома`
docker compose down -v


`# Если нужно только один из контейнеров`
docker compose up -d adminer


Очистка докера:

`# Очистить кеш сборки (кеш может занимать много места)`
docker builder prune


`# Удалить все неиспользуемые данные`
docker system prune -a


`# Удалить все неиспользуемые данные + тома`
docker system prune -a --volumes


`# Удалить все сети`
docker network prune


`# Удалить все тома`
docker volume prune


`# Удалить информацию о сборках`
docker buildx prune --builder desktop-linux


Тома
`# Просмотр ТОМОВ (а не смонтированных директорий, это разные вещи)`
docker volume
docker volume ls


`# Доступ через контейнер Busybox`
docker run --rm -it -v postgres_data:/data busybox sh


`# Резервное копирование тома базы данных в архив`
docker run --rm -v postgres_volume:/volume -v $(pwd):/backup alpine tar czvf /backup/postgres_backup.tar.gz -C /volume .


`# Резервное копирование тома базы media в архив`
docker run --rm -v media_volume:/volume -v $(pwd):/backup alpine tar czvf /backup/media_backup.tar.gz -C /volume .


`# Восстановление тома postgres`
docker run --rm -v postgres_volume:/volume -v $(pwd):/backup alpine sh -c "cd /volume && tar xzvf /backup/postgres_backup.tar.gz --strip 1"


`# Восстановление тома media`
docker run --rm -v media_volume:/volume -v $(pwd):/backup alpine sh -c "cd /volume && tar xzvf /backup/media_backup.tar.gz --strip 1"



