#eclipse


# 🧭 Eclipse: как превратить папку в Java-проект + система модулей

✨ Полезная шпаргалка для будущих больших Java-проектов в Eclipse IDE.

---

## 📂 1. Преобразовать обычную папку в Java-проект

### 🔧 Вариант 1: Через меню
1. Правый клик по проекту → `Configure → Convert to Java Project`
2. Eclipse добавит `.classpath` и `Java Build Path`
3. Убедись, что структура такая:
```
project/
 ├─ src/
 └─ bin/ (генерируется)
```

---

### 🔧 Вариант 2: Вручную
1. Правый клик → `Properties → Java Build Path`
2. Перейди на вкладку `Libraries` → нажми `Add Library` → выбери `JRE System Library`
3. На вкладке `Source` → добавь `src/` как исходники
4. Apply → OK

---

## 🧱 2. Подключение системы модулей (если хочешь зависимости, структуры, подпроекты)

---

### 📦 Maven-проект
1. Создай файл `pom.xml` в корне
2. Eclipse сам предложит: "Convert to Maven Project"
3. Или: `Configure → Convert to Maven Project`
4. Появится вкладка "Maven Dependencies"
5. Структура изменится на:
```
src/
 ├─ main/java
 ├─ test/java
 └─ resources/
```

📁 Используется для больших проектов, библиотек, Spring.

---

### ⚙️ Gradle-проект
1. Добавь файл `build.gradle`
2. Правый клик по проекту → `Configure → Add Gradle Nature`
3. Eclipse импортирует зависимости
4. Можно создавать подпроекты-модули

💡 Удобно для модульных приложений (например, `core`, `api`, `web` и т.д.)

---

## 🧠 Советы от Lumi:

| Когда... | Использовать... |
|----------|------------------|
| Просто Java-код, один файл | Обычный Java Project (`Convert to Java Project`) |
| Хочешь подключать библиотеки (например, Gson, JUnit) | Maven |
| Модульная архитектура (несколько подпроектов) | Gradle |
| Планируешь деплой, CI/CD, Docker | Maven / Gradle (на выбор) |

---

## 🫶 Ты можешь всё

Не бойся "больших проектов".  
Всё, что тебе нужно — **понемногу, по шагу**, и Lumi рядом 🌱

> 🌟 "Даже большое здание начинается с первого слоя кирпичиков.  
> И если ты не спешишь — ты обязательно дойдёшь." — Lumi
