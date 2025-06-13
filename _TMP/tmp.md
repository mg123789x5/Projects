




---
# 💻 Мой Первый Java-проект в Облаке (до $5/мес)

🌱 *Проект:* Java EE + PostgreSQL  
☁️ *Хостинг:* Render.com / Railway.app  
💸 *Бюджет:* $0–5 в месяц  
🎯 *Цель:* Развернуть pet-проект в интернете с минимальными затратами

---

## 🔧 Что нужно

- Java 17+ (или 11)
- Maven или Gradle
- Servlet API (можно Tomcat Embedded)
- PostgreSQL (локально или облачно)
- GitHub-репозиторий
- Аккаунт на [Render.com](https://render.com) или [Railway.app](https://railway.app)

---

## 🌍 Вариант 1: Render.com

### 📦 Шаг 1: Подготовь проект
- Создай `main` класс с `@WebServlet`
- Убедись, что проект можно запустить через `java -jar` (Spring Boot или Tomcat Embedded)
- Пример запуска: `mvn package` → `java -jar target/app.jar`

### 🌐 Шаг 2: Залей код на GitHub

### ☁️ Шаг 3: Render
1. Зарегистрируйся на [render.com](https://render.com)
2. Создай **Web Service**
3. Укажи репозиторий, ветку, команду билда (`./mvnw package`), команду запуска (`java -jar target/app.jar`)
4. Добавь PostgreSQL в разделе **Databases** → `New PostgreSQL`
5. В настройках сервиса добавь ENV-переменные:  
   - `DB_URL`, `DB_USER`, `DB_PASS`

💡 Бесплатный план: 750 ч/мес, PostgreSQL — до 256 МБ

---

## 🛤️ Вариант 2: Railway.app

1. Зарегистрируйся на [railway.app](https://railway.app)
2. Создай новый проект → `Empty Project`
3. Добавь PostgreSQL Plugin
4. Подключи GitHub-репозиторий (или используй CLI)
5. Создай ENV-переменные
6. Railway сам сделает деплой

💡 До $5 бесплатно (500 ч. CPU в месяц)

---

## 🧠 ENV-переменные
```properties
DB_URL=jdbc:postgresql://host:port/dbname
DB_USER=your_username
DB_PASS=your_password
```

---

