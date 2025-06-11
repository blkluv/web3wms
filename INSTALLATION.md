# Руководство по установке и запуску

## 🎯 Цель документа

Данное руководство поможет вам развернуть и запустить систему управления складом и отслеживания оборудования на вашем локальном компьютере или сервере.

## 📋 Системные требования

### Минимальные требования

- **ОС**: Windows 10+, macOS 10.15+, Ubuntu 18.04+ или другой Linux дистрибутив
- **RAM**: 8 GB (рекомендуется 16 GB)
- **Дисковое пространство**: 10 GB свободного места
- **CPU**: Intel Core i5 или AMD Ryzen 5 (2+ ядра)

### Необходимое ПО

#### Обязательно для запуска

- **Docker Desktop 4.0+** - [Скачать](https://www.docker.com/products/docker-desktop)
- **Docker Compose 2.0+** (обычно входит в Docker Desktop)
- **Git** - [Скачать](https://git-scm.com/downloads)

#### Для разработки (опционально)

- **Node.js 18+** - [Скачать](https://nodejs.org/en/download/)
- **Go 1.21+** - [Скачать](https://golang.org/dl/)
- **Visual Studio Code** или другая IDE

## 🚀 Пошаговая установка

### Шаг 1: Установка Docker

#### Windows

1. Скачайте Docker Desktop с официального сайта
2. Запустите установщик и следуйте инструкциям
3. Перезагрузите компьютер после установки
4. Запустите Docker Desktop и дождитесь полной загрузки

#### macOS

1. Скачайте Docker Desktop для Mac
2. Перетащите Docker в папку Applications
3. Запустите Docker из папки Applications
4. Предоставьте необходимые разрешения системы

#### Linux (Ubuntu/Debian)

```bash
# Обновление пакетов
sudo apt update

# Установка необходимых пакетов
sudo apt install apt-transport-https ca-certificates curl software-properties-common

# Добавление официального GPG ключа Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Добавление репозитория Docker
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Установка Docker
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io

# Установка Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Добавление пользователя в группу docker
sudo usermod -aG docker $USER

# Перезайдите в систему или выполните:
newgrp docker
```

### Шаг 2: Проверка установки

```bash
# Проверка Docker
docker --version
# Ожидаемый вывод: Docker version 24.0.x

# Проверка Docker Compose
docker-compose --version
# Ожидаемый вывод: Docker Compose version 2.x.x
```

### Шаг 3: Клонирование проекта

```bash
# Клонирование репозитория
git clone https://github.com/yourusername/warehouse-management-system.git

# Переход в директорию проекта
cd warehouse-management-system
```

### Шаг 4: Настройка окружения (опционально)

Создайте файл `.env` для кастомизации настроек:

```bash
# Создание файла окружения
cp .env.example .env
```

Пример содержимого `.env`:

```env
# Базы данных
MONGO_URI=mongodb://mongo:27017
POSTGRES_URI=postgresql://postgres:password@postgres:5432/warehouse

# Безопасность
JWT_SECRET=your_super_secret_jwt_key_change_in_production
ENCRYPTION_KEY=your_encryption_key_32_characters_long

# External APIs
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASS=your-app-password

# Feature flags
ENABLE_BLOCKCHAIN=true
ENABLE_NOTIFICATIONS=true
ENABLE_ANALYTICS=false
```

## 🚀 Запуск системы

### Автоматический запуск (рекомендуется)

```bash
# Сделать скрипт исполняемым
chmod +x start-system.sh

# Запуск всей системы
./start-system.sh
```

Этот скрипт автоматически:

- Создаст необходимые Docker сети
- Запустит все сервисы в правильном порядке
- Дождется готовности баз данных
- Загрузит демонстрационные данные
- Выведет информацию о доступных URL

### Ручной запуск

```bash
# Создание Docker сети
docker network create warehouse-network

# Запуск инфраструктурных сервисов
docker-compose up -d mongo rabbitmq ethereum-node

# Ожидание готовности баз данных (30-60 секунд)
sleep 60

# Запуск backend сервисов
docker-compose up -d auth-service warehouse-service tracking-service notification-service

# Ожидание готовности API (30 секунд)
sleep 30

# Запуск frontend
docker-compose up -d frontend

# Просмотр логов всех сервисов
docker-compose logs -f
```

### Проверка статуса

```bash
# Использование встроенного скрипта
./check-status.sh

# Или проверка вручную
docker-compose ps
```

Ожидаемый вывод:

```
Name                    Command               State                    Ports
-------------------------------------------------------------------------------------------
mvp_auth-service_1      ./auth-service                Up      0.0.0.0:8000->8000/tcp
mvp_frontend_1          docker-entrypoint.sh npm ...  Up      0.0.0.0:80->3000/tcp
mvp_mongo_1             docker-entrypoint.sh mongod   Up      0.0.0.0:27017->27017/tcp
mvp_notification-service_1  ./notification-service    Up      0.0.0.0:8003->8003/tcp
mvp_rabbitmq_1          docker-entrypoint.sh rabbitmq Up      4369/tcp, 5671/tcp, 0.0.0.0:5672->5672/tcp
mvp_tracking-service_1  docker-entrypoint.sh npm ...  Up      0.0.0.0:8002->8002/tcp
mvp_warehouse-service_1 ./warehouse-service           Up      0.0.0.0:8001->8001/tcp
```

## 🌐 Доступ к системе

### Основные URL

| Сервис               | URL                   | Описание               |
| -------------------- | --------------------- | ---------------------- |
| **Веб-интерфейс**    | http://localhost      | Главное приложение     |
| **Auth API**         | http://localhost:8000 | API аутентификации     |
| **Warehouse API**    | http://localhost:8001 | API управления складом |
| **Tracking API**     | http://localhost:8002 | API отслеживания       |
| **Notification API** | http://localhost:8003 | API уведомлений        |

### API документация (Swagger)

| Сервис               | Swagger URL                     |
| -------------------- | ------------------------------- |
| Auth Service         | http://localhost:8000/swagger/  |
| Warehouse Service    | http://localhost:8001/swagger/  |
| Tracking Service     | http://localhost:8002/api-docs/ |
| Notification Service | http://localhost:8003/swagger/  |

### Административные панели

| Сервис                  | URL                       | Логин/Пароль |
| ----------------------- | ------------------------- | ------------ |
| **RabbitMQ Management** | http://localhost:15672    | guest/guest  |
| **MongoDB**             | mongodb://localhost:27017 | -            |

### Тестовые учетные записи

```
👑 Администратор:
Email: admin@warehouse.local
Пароль: admin123

👨‍💼 Менеджер склада:
Email: manager@warehouse.local
Пароль: manager123

👷 Оператор склада:
Email: operator@warehouse.local
Пароль: operator123

👁️ Только просмотр:
Email: viewer@warehouse.local
Пароль: viewer123
```

## 🧪 Загрузка тестовых данных

### Автоматическая загрузка

```bash
# Загрузка демонстрационных данных
./demo-data-setup.sh
```

### Ручная загрузка

```bash
# Загрузка данных через Node.js скрипт
cd tracking-service-express
node demo-data-script.js

# Или через API calls
curl -X POST http://localhost:8000/api/demo-data
curl -X POST http://localhost:8001/api/demo-data
curl -X POST http://localhost:8002/api/demo-data
```

### Что включают тестовые данные

- **10 пользователей** с разными ролями
- **50+ единиц оборудования** различных категорий
- **100+ транзакций** склада
- **20+ накладных** (приходных и расходных)
- **15+ передач оборудования** с blockchain записями
- **Графики обслуживания** оборудования
- **Категории и справочники**

## 🔧 Устранение неполадок

### Проблема: Порты заняты

**Симптомы**: Ошибки типа "port 8000 already in use"

**Решение**:

```bash
# Проверить какие процессы используют порты
netstat -tulpn | grep :8000
# или на macOS:
lsof -i :8000

# Остановить конфликтующие процессы
sudo kill -9 <PID>

# Или изменить порты в docker-compose.yml
```

### Проблема: Недостаточно памяти

**Симптомы**: Контейнеры падают с OOMKilled

**Решение**:

```bash
# Увеличить память для Docker Desktop (в настройках)
# Или остановить ненужные приложения

# Проверить использование памяти
docker stats

# Очистить неиспользуемые образы
docker system prune -a
```

### Проблема: База данных не готова

**Симптомы**: Ошибки подключения к MongoDB

**Решение**:

```bash
# Проверить статус MongoDB
docker-compose logs mongo

# Перезапустить только MongoDB
docker-compose restart mongo

# Дождаться готовности (может занять 1-2 минуты)
docker-compose logs -f mongo | grep "waiting for connections"
```

### Проблема: Frontend не загружается

**Симптомы**: "This site can't be reached" на http://localhost

**Решение**:

```bash
# Проверить статус frontend контейнера
docker-compose logs frontend

# Перестроить frontend
docker-compose build frontend
docker-compose up -d frontend

# Очистить кэш браузера (Ctrl+F5)
```

### Проблема: API недоступны

**Симптомы**: Ошибки 502/503 при обращении к API

**Решение**:

```bash
# Проверить статус всех сервисов
./check-status.sh

# Перезапустить проблемный сервис
docker-compose restart auth-service

# Проверить логи
docker-compose logs -f auth-service
```

## 🔄 Управление системой

### Остановка системы

```bash
# Остановка всех сервисов
docker-compose down

# Остановка с удалением volumes (ОСТОРОЖНО: удалит все данные)
docker-compose down -v

# Остановка конкретного сервиса
docker-compose stop frontend
```

### Перезапуск сервисов

```bash
# Полный перезапуск
./clean-and-restart.sh

# Перезапуск без пересборки
docker-compose restart

# Перезапуск конкретного сервиса
docker-compose restart warehouse-service
```

### Обновление системы

```bash
# Получение последних изменений
git pull origin main

# Пересборка и перезапуск
./clean-and-rebuild.sh
```

### Просмотр логов

```bash
# Логи всех сервисов
docker-compose logs -f

# Логи конкретного сервиса
docker-compose logs -f auth-service

# Логи с ограничением по времени
docker-compose logs --since="2h" frontend
```

## 📊 Мониторинг и производительность

### Проверка ресурсов

```bash
# Использование ресурсов контейнерами
docker stats

# Дисковое пространство
docker system df

# Размер образов
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"
```

### Очистка системы

```bash
# Очистка неиспользуемых ресурсов
docker system prune

# Полная очистка (ОСТОРОЖНО!)
docker system prune -a --volumes
```

## 🆘 Получение помощи

### Проверочный список

- [ ] Docker запущен и работает корректно
- [ ] Все необходимые порты свободны
- [ ] Достаточно свободной памяти (8+ GB)
- [ ] Интернет соединение активно (для скачивания образов)
- [ ] Антивирус не блокирует Docker

### Сбор диагностической информации

```bash
# Информация о системе
docker version
docker-compose version
docker info

# Статус сервисов
docker-compose ps
./check-status.sh

# Логи всех сервисов
docker-compose logs > system-logs.txt
```

### Контакты для поддержки

📧 **Email**: your.email@example.com  
🐛 **Issues**: [GitHub Issues](https://github.com/yourusername/warehouse-management-system/issues)  
📖 **Документация**: [Wiki](https://github.com/yourusername/warehouse-management-system/wiki)

---

## ✅ Проверка успешной установки

После запуска системы проверьте:

1. **Веб-интерфейс доступен**: http://localhost
2. **Успешный вход**: используя admin@warehouse.local / admin123
3. **API отвечают**: http://localhost:8000/health
4. **Данные загружены**: видны тестовые товары и оборудование
5. **Уведомления работают**: появляются в интерфейсе

🎉 **Поздравляем! Система успешно установлена и готова к использованию!**
