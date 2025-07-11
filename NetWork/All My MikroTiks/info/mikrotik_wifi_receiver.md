# 📡 MikroTik как Wi-Fi приёмник (шпаргалка)
> Дата: 2025-07-10
> Назначение: Настроить роутер MikroTik как приёмник Wi-Fi, который передаёт интернет в LAN

---

## 🎯 Цель
Подключить MikroTik к Wi-Fi как клиент (режим `station`) и раздавать интернет по кабелю (LAN)

---

## 🔧 Шаги настройки

### 1. Настройка Wi-Fi интерфейса
- Открыть **Wireless → Interfaces**
- Двойной клик на `wlan1`
- Установить:
  - `Mode = station`
  - `SSID = [Имя сети Wi-Fi]`
  - `Frequency Mode = regulatory-domain`
  - `Country = [ваша страна]`
  - `Security Profile = [созданный профиль]`

### 2. Создание Security Profile
- Wireless → **Security Profiles**
- `+` Новый профиль:
  - `Name`: любое имя
  - `WPA2 Pre-Shared Key`: пароль от Wi-Fi
  - Включить WPA2

### 3. DHCP client для wlan1
- IP → **DHCP Client**
- `+` → Interface: `wlan1`
- Опции: ✅ Add Default Route, ✅ Use Peer DNS

### 4. Настройка LAN
- IP → **Addresses**
  - Добавить IP для `ether2`, например `192.168.88.1/24`
- IP → **DHCP Server**
  - Удалить старые DHCP-сервера
  - `+` Новый:
    - Interface: `ether2`
    - Subnet: `192.168.88.0/24`
    - Gateway: `192.168.88.1`
    - DNS: `8.8.8.8`

### 5. NAT (чтобы интернет шёл в LAN)
- IP → **Firewall** → вкладка `NAT`
- `+` Новое правило:
  - `Chain = srcnat`
  - `Out Interface = wlan1`
  - `Action = masquerade`

---

## ✅ Готово!
Теперь MikroTik ловит Wi-Fi и раздаёт его по кабелю!
