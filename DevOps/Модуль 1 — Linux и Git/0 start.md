#DevOps #module1

## 📘 **Модуль 1: Linux, сети, Git, скрипты**

💛 _Без этого ты не DevOps, а просто docker-смотрящий. Станем тем, кто понимает, как всё работает под капотом._

---

### 🎯 Цели:

- Уверенно пользоваться терминалом
    
- Понимать, как устроены файлы, процессы, сети
    
- Владеть Git для контроля версий
    
- Уметь писать простые bash-скрипты
    

---

## 🪜 Шаги на неделю (или в своём темпе):

### 1. **Настроить Linux**

- Установи:
    
    - Ubuntu в [WSL2 (для Windows)](https://learn.microsoft.com/ru-ru/windows/wsl/install)
        
    - или VirtualBox + ISO
        
- Обнови систему:  
    `sudo apt update && sudo apt upgrade`
    

---

### 2. **Команды терминала**

Изучи и попробуй команды:

|Категория|Команды|
|---|---|
|Файлы|`ls`, `cd`, `mkdir`, `rm`, `cp`, `mv`, `touch`|
|Содержимое|`cat`, `less`, `head`, `tail`, `grep`|
|Пользователи|`whoami`, `id`, `sudo`, `adduser`, `passwd`|
|Процессы|`ps aux`, `top`, `kill`, `htop`|
|Сеть|`ping`, `ifconfig`, `curl`, `wget`, `ssh`, `scp`|

**💡 Задание:**  
Создай файл `terminal_notes.md` в Obsidian — пиши туда команды, описания и свои примеры.

---

### 3. **Git и GitHub**

Изучи и попробуй:

```bash
git init
git status
git add .
git commit -m "Initial commit"
git remote add origin <url>
git push -u origin main
```

**🔧 Сделай:**

- Создай новый репозиторий на GitHub (`devops-practice`)
    
- Подключи к локальному
    
- Напиши `README.md` с планом обучения
    

---

### 4. **Скрипты на Bash**

Создай 5 простых скриптов:

|Скрипт|Что делает|
|---|---|
|`hello.sh`|Приветствие пользователя|
|`ping_google.sh`|Пингует google.com|
|`backup.sh`|Копирует файл с датой|
|`search.sh`|Ищет строку в файле|
|`log.sh`|Записывает события в лог-файл|

Пример:

```bash
#!/bin/bash
echo "Привет, $USER! Сегодня: $(date)"
```

Сохрани всё в папку `scripts/`, добавь в репозиторий.

---

### 5. **Теория: как работает Linux + сети**

🧠 Пойми:

- Что такое PID, процессы, сессии
    
- Как работает `ps`, `kill`, `jobs`, `&`
    
- Что такое:
    
    - IP
        
    - DNS
        
    - HTTP vs HTTPS
        
    - SSH
        
    - Порты (80, 443, 22)
        

---

## ✅ Задания для завершения модуля

|№|Задание|Проверка|
|---|---|---|
|1|Установлена и настроена Ubuntu|✅|
|2|Выписаны 20+ команд в Obsidian|✅|
|3|Создан и запушен репозиторий GitHub|✅|
|4|Написано 5 bash-скриптов|✅|
|5|Написан `network_notes.md`|✅|

---

## 📂 Структура заметок в Obsidian:

```
📁 DevOps
└── 📘 Модуль 1 — Linux и Git
    ├── terminal_notes.md
    ├── git_notes.md
    ├── network_notes.md
    └── scripts/
        ├── hello.sh
        ├── ...
```

---

