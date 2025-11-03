# AmneziaWG Client Docker Setup

Docker-контейнер для клиентской части AmneziaWG с поддержкой обфускации трафика.

## Структура файлов

```
.
├── docker-compose.yml      # Конфигурация Docker Compose
├── Dockerfile              # Сборка образа из исходников
├── client.conf            # Ваш конфиг AmneziaWG (создайте сами)
└── client.conf.example    # Пример конфигурации
```

## Быстрый старт

### 1. Создайте конфигурационный файл

```bash
cp client.conf.example client.conf
nano client.conf  # Отредактируйте под свои параметры
```

### 2. Соберите и запустите контейнер

```bash
# Сборка образа (может занять несколько минут)
docker-compose build

# Запуск контейнера
docker-compose up -d

# Просмотр логов
docker-compose logs -f
```

### 3. Проверка подключения

```bash
# Статус интерфейса
docker exec amneziawg-client wg show

# Проверка IP-адреса
docker exec amneziawg-client ip addr show wg0

# Проверка внешнего IP
curl ifconfig.me
```

## Управление

```bash
# Остановить контейнер
docker-compose down

# Перезапустить
docker-compose restart

# Просмотр логов
docker-compose logs -f amneziawg-client

# Пересборка после изменений
docker-compose build --no-cache
docker-compose up -d
```

## Отладка

Для включения расширенного логирования измените в `docker-compose.yml`:

```yaml
environment:
  - LOG_LEVEL=debug
```

## Особенности

- **Network mode: host** - контейнер использует сетевой стек хоста
- **Автоматический запуск** - контейнер перезапускается при перезагрузке системы
- **Обфускация трафика** - поддержка всех параметров AmneziaWG (Jc, Jmin, Jmax, S1, S2, H1-H4)

## Требования

- Docker 20.10+
- Docker Compose 1.29+
- Linux kernel с поддержкой TUN/TAP

## Решение проблем

### Контейнер не запускается

Проверьте права доступа:
```bash
chmod 600 client.conf
```

### VPN подключается, но нет интернета

Проверьте AllowedIPs в конфиге:
```
AllowedIPs = 0.0.0.0/0, ::/0
```

### Проблемы с DNS

Добавьте в client.conf:
```
DNS = 1.1.1.1, 8.8.8.8
```
