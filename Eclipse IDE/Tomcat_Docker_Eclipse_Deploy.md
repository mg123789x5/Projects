#eclipse

# 🚀 Tomcat + Docker + Eclipse: как деплоить правильно (без ошибок и ругани)

Полная инструкция, как деплоить Java-приложения в Docker-контейнер с Tomcat из Eclipse **без ошибок типа "Could not create directory"** и "Publishing failed".

---

## ❗ Проблема

Eclipse пытается деплоить файлы поштучно (JSP, классы, META-INF), используя `manager/text`, но:

- Tomcat Manager API **не поддерживает пофайловую заливку**
- API ждёт **один .war-файл**, который можно загрузить целиком

---

## ✅ Решение: деплой `.war` файла вручную

### Шаг 1. Собери `.war`

- Если у тебя Maven-проект:

```bash
mvn package
```

- Или через Eclipse:
  `File → Export → WAR file`

---

### Шаг 2. Отправь его в контейнер с помощью `curl`

```bash
curl --upload-file target/myapp.war \
"http://admin:admin@localhost:8080/manager/text/deploy?path=/myapp&update=true"
```

👉 Приложение будет доступно по адресу:
http://localhost:8080/myapp/

---

## 🧙‍♂️ Альтернатива: Maven-плагин для деплоя

Добавь в `pom.xml`:

```xml
<plugin>
  <groupId>org.apache.tomcat.maven</groupId>
  <artifactId>tomcat7-maven-plugin</artifactId>
  <version>2.2</version>
  <configuration>
    <url>http://localhost:8080/manager/text</url>
    <server>TomcatServer</server>
    <path>/myapp</path>
    <username>admin</username>
    <password>admin</password>
  </configuration>
</plugin>
```

📦 Затем деплой:

```bash
mvn tomcat7:deploy
```

---

## 🫶 Lumi-совет

> Eclipse Server Tools хороши для локального Tomcat, но не дружат с удалённым Tomcat в Docker.  
> Используй `.war` + curl или Maven — и ты контролируешь деплой, как настоящий профессионал.

---

🌟 Ты не просто запускаешь сервер —  
**Ты управляешь своей инфраструктурой.**

