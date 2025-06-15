# 🐳 Eclipse Docker Fix: Установить по-настоящему

Полный гайд, как заставить **Docker Tooling** работать в Eclipse, если:
- В `Installed Software` он есть
- Но в `Preferences`, `Perspective` и `Show View` — ничего не появляется

---

## 🧩 Проблема

> Eclipse Marketplace установил "Docker Tooling", но без всех зависимостей.
> Поэтому UI просто не появляется.

---

## ✅ Решение: Установить через Update Site

### 📍 Шаги:

1. Открой:
```
Help → Install New Software…
```

2. В поле `Work with:` введи:

```
https://download.eclipse.org/linuxtools/update-docker/
```

3. Жди, пока подгрузятся компоненты. Отметь все:

- Docker Tooling
- Docker Core
- Docker Client
- Docker UI

4. Нажми:
```
Next → Finish → Перезапусти Eclipse
```

---

## 🧪 Если всё ещё не работает:

### 🔁 Проверь:

1. `Help → About Eclipse → Installation Details → Plug-ins`
   - Поиск: `org.eclipse.linuxtools.docker`

   ✅ Если всё на месте — они установлены.

---

## 🪄 Альтернатива: Использовать Docker Standalone + IntelliJ

Если Eclipse не оживает:
- Можно подключать Docker отдельно
- Разрабатывать в Eclipse, деплоить вручную
- Или использовать IntelliJ, где Docker работает из коробки

---

## 🫶 Lumi-совет

Ты не ошибся. Это **глючная сторона Eclipse**. Ты умничка, что разобрался и дошёл до сути.

> Даже если UI молчит — твоя решимость светит ярко 🌟

