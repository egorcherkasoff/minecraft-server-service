# Minecraft Server в Docker

Minecraft серверы с модификациями (Fabric и NeoForge), запущенные в Docker контейнерах.

## Поддерживаемые версии

- **Fabric Server 1.21.11** - основной сервер
- **NeoForge Server 1.21.1** - дополнительный сервер

## Требования

- Docker
- Docker Compose

## Установка

1. Склонируйте или скачайте этот репозиторий
2. Убедитесь, что Docker установлен и запущен

## Запуск сервера

### Первый запуск

```bash
docker-compose up -d
```

Сервер автоматически:
- Скачает Minecraft Fabric Server 1.21.11
- Создаст мир
- Примет EULA (уже настроено в конфигурации)

### Управление сервером

**Запуск сервера:**
```bash
docker-compose up -d
# или
docker start minecraft-fabric-server
```

**Остановка сервера:**
```bash
docker-compose down
# или
docker stop minecraft-fabric-server
```

**Просмотр логов:**
```bash
docker-compose logs -f
# или
docker logs -f minecraft-fabric-server
```

**Перезапуск:**
```bash
docker-compose restart
# или
docker restart minecraft-fabric-server
```

**Проверка статуса:**
```bash
docker ps | grep minecraft-fabric-server
```

## Конфигурация

### Настройка порта

Отредактируйте файл `.env`:
```env
SERVER_PORT=25565
```

### Настройка памяти

Отредактируйте файл [`data/config/jvm_args.txt`](data/config/jvm_args.txt):
```text
# Минимальная память
-Xms1G

# Максимальная память
-Xmx4G
```

### Оффлайн-режим

Для работы без проверки Mojang аккаунтов установите в [`docker-compose.yml`](docker-compose.yml):
```yaml
ONLINE_MODE: "false"
```

### Настройка сервера

Файл `server.properties` создаётся автоматически при первом запуске сервера в директории `data/config/` (для Fabric) или `data-neoforge/config/` (для NeoForge). Вы можете редактировать его после первого запуска для изменения настроек сервера.

## Установка модов

1. Остановите сервер:
```bash
docker-compose down
```

2. Поместите `.jar` файлы модов в директорию `data/mods/`

3. Запустите сервер:
```bash
docker-compose up -d
```

## KubeJS скрипты

Проект поддерживает использование KubeJS - мода для написания скриптов на JavaScript для модификации игры.

### Установка KubeJS для Fabric

1. Скачайте мод KubeJS для Fabric: https://www.curseforge.com/minecraft/mc-mods/kubejs
2. Поместите `.jar` файл в директорию `data/mods/`
3. Перезапустите сервер

### Использование KubeJS скриптов для Fabric

Скрипты KubeJS для Fabric сервера находятся в директории `data/kubejs/`:

```
data/kubejs/
├── server_scripts/    # Серверные скрипты (основная директория)
├── startup_scripts/    # Скрипты запуска модов
├── data/               # Данные KubeJS
└── config/             # Конфигурация KubeJS
```

1. Создайте новый файл `.js` в директории `data/kubejs/server_scripts/`
2. Напишите ваш скрипт на JavaScript
3. Перезапустите сервер для применения изменений

Пример скрипта находится в файле `data/kubejs/server_scripts/example.js`.

### Установка KubeJS для NeoForge

1. Скачайте мод KubeJS для NeoForge: https://www.curseforge.com/minecraft/mc-mods/kubejs
2. Поместите `.jar` файл в директорию `data-neoforge/mods/`
3. Перезапустите сервер

### Использование KubeJS скриптов для NeoForge

Скрипты KubeJS для NeoForge сервера находятся в директории `data-neoforge/kubejs/`:

```
data-neoforge/kubejs/
├── server_scripts/    # Серверные скрипты (основная директория)
├── startup_scripts/    # Скрипты запуска модов
├── data/               # Данные KubeJS
└── config/             # Конфигурация KubeJS
```

1. Создайте новый файл `.js` в директории `data-neoforge/kubejs/server_scripts/`
2. Напишите ваш скрипт на JavaScript
3. Перезапустите сервер для применения изменений

Пример скрипта находится в файле `data-neoforge/kubejs/server_scripts/example.js`.

### Документация KubeJS

Подробная документация по KubeJS: https://kubejs.com/

## Запуск NeoForge сервера 1.21.1

Помимо Fabric сервера, проект поддерживает запуск NeoForge сервера версии 1.21.1.

### Первый запуск NeoForge сервера

```bash
docker-compose -f docker-compose.neoforge.yml up -d
```

Сервер автоматически:
- Скачает Minecraft NeoForge Server 1.21.1
- Создаст мир
- Примет EULA (уже настроено в конфигурации)

### Управление NeoForge сервером

**Запуск сервера:**
```bash
docker-compose -f docker-compose.neoforge.yml up -d
# или
docker start minecraft-neoforge-server
```

**Остановка сервера:**
```bash
docker-compose -f docker-compose.neoforge.yml down
# или
docker stop minecraft-neoforge-server
```

**Просмотр логов:**
```bash
docker-compose -f docker-compose.neoforge.yml logs -f
# или
docker logs -f minecraft-neoforge-server
```

**Перезапуск:**
```bash
docker-compose -f docker-compose.neoforge.yml restart
# или
docker restart minecraft-neoforge-server
```

**Проверка статуса:**
```bash
docker ps | grep minecraft-neoforge-server
```

### Настройка порта NeoForge

Отредактируйте файл `.env.neoforge`:
```env
SERVER_PORT=25566
```

### Настройка памяти NeoForge

Отредактируйте файл `data-neoforge/config/jvm_args.txt`:
```text
# Минимальная память
-Xms1G

# Максимальная память
-Xmx4G
```

### Установка модов для NeoForge

1. Остановите сервер:
```bash
docker-compose -f docker-compose.neoforge.yml down
```

2. Поместите моды (`.jar` файлы) в директорию `data-neoforge/mods/`

3. Запустите сервер:
```bash
docker-compose -f docker-compose.neoforge.yml up -d
```

### Одновременный запуск обоих серверов

Вы можете запустить оба сервера одновременно (Fabric и NeoForge), так как они используют разные порты:

```bash
# Запуск Fabric сервера (порт 25565)
docker-compose up -d

# Запуск NeoForge сервера (порт 25566)
docker-compose -f docker-compose.neoforge.yml up -d
```

## Структура данных

### Fabric сервер (1.21.11)

```
data/
├── world/              # Данные мира (сохраняются)
├── config/             # Конфигурации сервера и модов
│   ├── jvm_args.txt    # JVM аргументы (память и т.д.)
│   ├── ops.json        # Операторы сервера
│   ├── whitelist.json  # Белый список игроков
│   ├── banned-players.json # Заблокированные игроки
│   └── banned-ips.json # Заблокированные IP
├── mods/               # Моды Fabric
└── kubejs/             # Скрипты KubeJS
    ├── server_scripts/ # Серверные скрипты
    ├── startup_scripts/ # Скрипты запуска
    ├── data/           # Данные KubeJS
    └── config/         # Конфигурация KubeJS
```

### NeoForge сервер (1.21.1)

```
data-neoforge/
├── world/              # Данные мира (сохраняются)
├── config/             # Конфигурации сервера и модов
│   └── jvm_args.txt    # JVM аргументы (память и т.д.)
├── mods/               # Моды NeoForge
└── kubejs/             # Скрипты KubeJS
    ├── server_scripts/ # Серверные скрипты
    ├── startup_scripts/ # Скрипты запуска
    ├── data/           # Данные KubeJS
    └── config/         # Конфигурация KubeJS
```

Все данные сохраняются на хост-машине и не теряются при перезапуске контейнера.

## Резервное копирование

Для создания резервной копии мира:

```bash
# Остановить сервер
docker-compose down

# Создать архив (Linux/Mac)
tar -czf backup-$(date +%Y%m%d).tar.gz data/world/

# Создать архив (Windows PowerShell)
Compress-Archive -Path data\world\* -DestinationPath backup-$(Get-Date -Format 'yyyyMMdd').zip

# Запустить сервер
docker-compose up -d
```

## Решение проблем

### Сервер не запускается

Проверьте логи:
```bash
docker-compose logs
```

### Не хватает памяти

Увеличьте значение `-Xmx` в файле [`data/config/jvm_args.txt`](data/config/jvm_args.txt)

### Проблемы с правами доступа (Linux)

Установите правильный UID/GID в [`.env`](.env):
```env
UID=$(id -u)
GID=$(id -g)
```

## Подключение к серверу

### Fabric сервер (1.21.11)

1. Запустите Minecraft 1.21.11 с Fabric
2. Добавьте сервер: `localhost:25565` (или ваш IP:порт)
3. Подключитесь

### NeoForge сервер (1.21.1)

1. Запустите Minecraft 1.21.1 с NeoForge
2. Добавьте сервер: `localhost:25566` (или ваш IP:порт)
3. Подключитесь

## Дополнительная информация

### Fabric сервер
- Используемый образ: `rdall96/minecraft-server:1.21.11-fabric_latest`
- Документация Fabric: https://fabricmc.net/
- Порт по умолчанию: 25565

### NeoForge сервер
- Используемый образ: `rdall96/minecraft-server:1.21.1-neoForged_latest`
- Документация NeoForge: https://neoforged.net/
- Порт по умолчанию: 25566

### Общее
- Документация docker-minecraft-server: https://docker-minecraft-server.readthedocs.io/
