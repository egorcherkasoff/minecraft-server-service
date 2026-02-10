# Minecraft Fabric Server 1.21.11 в Docker

Minecraft сервер версии 1.21.11 с Fabric модификациями, запущенный в Docker контейнере.

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

### Настройка сервера

Отредактируйте [`server.properties`](server.properties) для изменения настроек сервера.

### Оффлайн-режим

Для работы без проверки Mojang аккаунтов установите в [`docker-compose.yml`](docker-compose.yml):
```yaml
ONLINE_MODE: "false"
```

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

## Структура данных

```
data/
├── world/              # Данные мира (сохраняются)
├── config/             # Конфигурации сервера и модов
│   ├── jvm_args.txt    # JVM аргументы (память и т.д.)
│   ├── ops.json        # Операторы сервера
│   ├── whitelist.json  # Белый список игроков
│   ├── banned-players.json # Заблокированные игроки
│   └── banned-ips.json # Заблокированные IP
└── mods/               # Моды Fabric
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

1. Запустите Minecraft 1.21.11 с Fabric
2. Добавьте сервер: `localhost:25565` (или ваш IP:порт)
3. Подключитесь

## Дополнительная информация

- Используемый образ: `rdall96/minecraft-server:1.21.11-fabric_latest`
- Документация Fabric: https://fabricmc.net/
- Документация docker-minecraft-server: https://docker-minecraft-server.readthedocs.io/
