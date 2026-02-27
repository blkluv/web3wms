# API Референс

Полная документация API для всех микросервисов системы управления складом и отслеживания оборудования.

## 📋 Общие сведения

### Базовые URL

| Сервис               | URL                     | Порт |
| -------------------- | ----------------------- | ---- |
| Auth Service         | `http://localhost:8000` | 8000 |
| Warehouse Service    | `http://localhost:8001` | 8001 |
| Tracking Service     | `http://localhost:8002` | 8002 |
| Notification Service | `http://localhost:8003` | 8003 |

### Аутентификация

Все защищенные endpoints требуют JWT токен в заголовке:

```http
Authorization: Bearer <jwt_token>
```

### Общие форматы ответов

#### Успешный ответ

```json
{
  "success": true,
  "data": {
    // Данные ответа
  },
  "message": "Operation completed successfully"
}
```

#### Ошибка

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable error message",
    "details": {}
  }
}
```

### Коды состояния HTTP

- `200 OK` - Успешный запрос
- `201 Created` - Ресурс создан
- `400 Bad Request` - Ошибка в запросе
- `401 Unauthorized` - Требуется аутентификация
- `403 Forbidden` - Недостаточно прав
- `404 Not Found` - Ресурс не найден
- `500 Internal Server Error` - Внутренняя ошибка сервера

## 🔐 Auth Service API

### Endpoints

#### POST /login

Аутентификация пользователя

**Запрос:**

```json
{
  "email": "admin@warehouse.local",
  "password": "admin123"
}
```

**Ответ:**

```json
{
  "success": true,
  "data": {
    "user": {
      "id": "507f1f77bcf86cd799439011",
      "username": "admin",
      "email": "admin@warehouse.local",
      "role": "admin",
      "eth_address": "0x742d35Cc6aBb78532B123C1234567890AbCdEf12"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

#### POST /signup

Регистрация нового пользователя

**Запрос:**

```json
{
  "username": "newuser",
  "email": "newuser@example.com",
  "password": "password123",
  "first_name": "John",
  "last_name": "Doe",
  "role": "operator",
  "department": "Warehouse"
}
```

#### GET /profile

Получение профиля текущего пользователя

**Заголовки:**

```http
Authorization: Bearer <jwt_token>
```

**Ответ:**

```json
{
  "success": true,
  "data": {
    "id": "507f1f77bcf86cd799439011",
    "username": "admin",
    "email": "admin@warehouse.local",
    "first_name": "Admin",
    "last_name": "User",
    "role": "admin",
    "department": "IT",
    "eth_address": "0x742d35Cc6aBb78532B123C1234567890AbCdEf12",
    "created_at": "2024-01-01T12:00:00Z",
    "last_login": "2024-12-15T10:30:00Z"
  }
}
```

#### GET /users

Получение списка всех пользователей (только для админов)

**Query параметры:**

- `page` (int) - Номер страницы (по умолчанию: 1)
- `limit` (int) - Количество на странице (по умолчанию: 10)
- `role` (string) - Фильтр по роли

**Ответ:**

```json
{
  "success": true,
  "data": {
    "users": [...],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 25,
      "pages": 3
    }
  }
}
```

#### PUT /users/:id

Обновление пользователя

#### DELETE /users/:id

Удаление пользователя

#### POST /refresh

Обновление JWT токена

---

## 📦 Warehouse Service API

### Endpoints

#### GET /items

Получение списка товаров склада

**Query параметры:**

- `page` (int) - Номер страницы
- `limit` (int) - Количество на странице
- `category` (string) - Фильтр по категории
- `status` (string) - Фильтр по статусу
- `search` (string) - Поиск по названию или серийному номеру

**Ответ:**

```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "507f1f77bcf86cd799439012",
        "name": "Ноутбук Dell XPS 13",
        "serial_number": "DLL-XPS-13-2024-001",
        "category": "computers",
        "description": "13-дюймовый ультрабук",
        "manufacturer": "Dell Inc.",
        "price": 125000.5,
        "quantity": 15,
        "min_quantity": 3,
        "location": "Склад А-1, Стеллаж 2-3",
        "status": "available",
        "created_at": "2024-03-15T14:30:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 125,
      "pages": 13
    }
  }
}
```

#### POST /items

Добавление нового товара

**Запрос:**

```json
{
  "name": "Принтер HP LaserJet Pro",
  "serial_number": "HP-LJ-PRO-2024-001",
  "category": "printers",
  "description": "Монохромный лазерный принтер",
  "manufacturer": "HP Inc.",
  "price": 35000.0,
  "quantity": 5,
  "min_quantity": 2,
  "location": "Склад А-2, Стеллаж 1-1"
}
```

#### PUT /items/:id

Обновление товара

#### DELETE /items/:id

Удаление товара

#### GET /transactions

История транзакций склада

**Ответ:**

```json
{
  "success": true,
  "data": {
    "transactions": [
      {
        "id": "507f1f77bcf86cd799439013",
        "item_id": "507f1f77bcf86cd799439012",
        "transaction_type": "intake",
        "quantity": 5,
        "responsible_user": "admin",
        "reason": "Закупка нового оборудования",
        "date": "2024-03-15T14:30:00Z"
      }
    ]
  }
}
```

#### POST /transactions

Создание новой транзакции

#### GET /invoices

Список накладных

**Ответ:**

```json
{
  "success": true,
  "data": {
    "invoices": [
      {
        "id": "507f1f77bcf86cd799439014",
        "invoice_number": "INV-2024-001",
        "type": "incoming",
        "supplier": "ООО ТехПоставка",
        "total_amount": 250000.0,
        "status": "approved",
        "items": [
          {
            "item_id": "507f1f77bcf86cd799439012",
            "quantity": 2,
            "unit_price": 125000.0
          }
        ],
        "created_at": "2024-03-15T10:00:00Z",
        "approved_at": "2024-03-15T11:30:00Z"
      }
    ]
  }
}
```

#### POST /invoices

Создание новой накладной

#### GET /categories

Список категорий товаров

#### POST /categories

Создание новой категории

---

## 🔗 Tracking Service API

### Endpoints

#### GET /equipment

Список оборудования

**Ответ:**

```json
{
  "success": true,
  "data": [
    {
      "_id": "507f1f77bcf86cd799439015",
      "name": "Ноутбук Dell XPS 15",
      "serial_number": "DLL-XPS-15-2024-001",
      "category": "laptop",
      "current_owner": "admin",
      "status": "active",
      "blockchain_id": "0x1234567890abcdef",
      "location": "Офис IT отдела",
      "purchase_date": "2024-01-15T00:00:00Z",
      "warranty_expiry": "2027-01-15T00:00:00Z",
      "created_at": "2024-01-15T12:00:00Z"
    }
  ]
}
```

#### POST /equipment

Добавление нового оборудования

**Запрос:**

```json
{
  "name": "MacBook Pro 16",
  "serial_number": "MBP-16-2024-001",
  "category": "laptop",
  "description": "Ноутбук для разработки",
  "initial_owner": "developer1",
  "purchase_price": 280000.0,
  "warranty_months": 36
}
```

#### GET /equipment/:id

Получение информации об оборудовании

#### PUT /equipment/:id

Обновление оборудования

#### DELETE /equipment/:id

Удаление оборудования

#### GET /transfers

История передач оборудования

**Ответ:**

```json
{
  "success": true,
  "data": [
    {
      "_id": "507f1f77bcf86cd799439016",
      "equipment_id": "507f1f77bcf86cd799439015",
      "from_user": "admin",
      "to_user": "developer1",
      "transfer_date": "2024-03-20T14:00:00Z",
      "reason": "Назначение разработчику",
      "status": "completed",
      "blockchain_tx": "0xabcdef1234567890...",
      "created_at": "2024-03-20T14:00:00Z"
    }
  ]
}
```

#### POST /transfers

Создание новой передачи

**Запрос:**

```json
{
  "equipment_id": "507f1f77bcf86cd799439015",
  "from_user": "admin",
  "to_user": "developer1",
  "reason": "Назначение для проекта",
  "transfer_date": "2024-03-20T14:00:00Z"
}
```

#### GET /maintenance

График обслуживания

**Ответ:**

```json
{
  "success": true,
  "data": [
    {
      "_id": "507f1f77bcf86cd799439017",
      "equipment_id": "507f1f77bcf86cd799439015",
      "maintenance_type": "routine_inspection",
      "scheduled_date": "2024-04-15T10:00:00Z",
      "description": "Плановый осмотр и чистка",
      "responsible_technician": "tech1",
      "status": "scheduled",
      "created_at": "2024-03-01T12:00:00Z"
    }
  ]
}
```

#### POST /maintenance

Планирование обслуживания

#### GET /blockchain/contract-info

Информация о блокчейн контракте

**Ответ:**

```json
{
  "success": true,
  "data": {
    "contract_address": "0x742d35Cc6aBb78532B123C1234567890AbCdEf12",
    "network": "ganache",
    "block_number": 1234,
    "gas_price": "20000000000",
    "is_connected": true
  }
}
```

#### POST /blockchain/deploy

Развертывание нового контракта

#### GET /qr/:equipmentId

Генерация QR-кода для оборудования

---

## 🔔 Notification Service API

### Endpoints

#### GET /notifications

Получение уведомлений пользователя

**Query параметры:**

- `unread_only` (boolean) - Только непрочитанные
- `type` (string) - Тип уведомления
- `limit` (int) - Количество уведомлений

**Ответ:**

```json
{
  "success": true,
  "data": [
    {
      "id": "507f1f77bcf86cd799439018",
      "user_id": "507f1f77bcf86cd799439011",
      "type": "inventory_low_stock",
      "title": "Низкий остаток товара",
      "message": "Остаток ноутбуков Dell XPS 13 ниже минимального уровня",
      "is_read": false,
      "created_at": "2024-03-20T15:30:00Z",
      "data": {
        "item_id": "507f1f77bcf86cd799439012",
        "current_quantity": 2,
        "min_quantity": 3
      }
    }
  ]
}
```

#### POST /notifications

Создание нового уведомления

**Запрос:**

```json
{
  "user_id": "507f1f77bcf86cd799439011",
  "type": "equipment_transfer",
  "title": "Передача оборудования",
  "message": "Вам передан ноутбук Dell XPS 15",
  "data": {
    "equipment_id": "507f1f77bcf86cd799439015",
    "transfer_id": "507f1f77bcf86cd799439016"
  }
}
```

#### PUT /notifications/:id/read

Отметка уведомления как прочитанного

#### DELETE /notifications/:id

Удаление уведомления

#### GET /notifications/stats

Статистика уведомлений

**Ответ:**

```json
{
  "success": true,
  "data": {
    "total_notifications": 45,
    "unread_count": 8,
    "by_type": {
      "inventory_low_stock": 3,
      "equipment_transfer": 2,
      "maintenance_reminder": 2,
      "invoice_approval": 1
    }
  }
}
```

---

## 🧪 Примеры использования

### Полный workflow передачи оборудования

1. **Получение списка оборудования**

```bash
curl -X GET "http://localhost:8002/equipment" \
  -H "Authorization: Bearer $JWT_TOKEN"
```

2. **Создание передачи**

```bash
curl -X POST "http://localhost:8002/transfers" \
  -H "Authorization: Bearer $JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "equipment_id": "507f1f77bcf86cd799439015",
    "from_user": "admin",
    "to_user": "developer1",
    "reason": "Назначение для проекта"
  }'
```

3. **Проверка статуса в блокчейн**

```bash
curl -X GET "http://localhost:8002/blockchain/contract-info" \
  -H "Authorization: Bearer $JWT_TOKEN"
```

### Управление складскими операциями

1. **Добавление товара**

```bash
curl -X POST "http://localhost:8001/items" \
  -H "Authorization: Bearer $JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Монитор Samsung 27",
    "category": "monitors",
    "quantity": 10,
    "price": 25000.00
  }'
```

2. **Создание накладной**

```bash
curl -X POST "http://localhost:8001/invoices" \
  -H "Authorization: Bearer $JWT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "incoming",
    "supplier": "ООО ТехПоставка",
    "items": [
      {
        "item_id": "507f1f77bcf86cd799439012",
        "quantity": 5,
        "unit_price": 25000.00
      }
    ]
  }'
```

## 🔧 Коды ошибок

### Auth Service

- `AUTH_001` - Неверные учетные данные
- `AUTH_002` - Пользователь не найден
- `AUTH_003` - Токен истек
- `AUTH_004` - Недостаточно прав

### Warehouse Service

- `WH_001` - Товар не найден
- `WH_002` - Недостаточно товара на складе
- `WH_003` - Дублирующийся серийный номер
- `WH_004` - Недопустимая категория

### Tracking Service

- `TR_001` - Оборудование не найдено
- `TR_002` - Пользователь не может передать оборудование
- `TR_003` - Блокчейн транзакция не удалась
- `TR_004` - QR-код не может быть сгенерирован

### Notification Service

- `NOT_001` - Уведомление не найдено
- `NOT_002` - Недопустимый тип уведомления
- `NOT_003` - Пользователь не найден

## 📊 Rate Limiting

Все API endpoints имеют ограничения по количеству запросов:

- **Общие endpoint**: 100 запросов в минуту
- **Аутентификация**: 10 попыток в минуту
- **Создание записей**: 50 запросов в минуту
- **Поиск**: 200 запросов в минуту

При превышении лимита возвращается статус `429 Too Many Requests`.

## 🔄 Версионирование API

Текущая версия API: **v1**

Для получения самой актуальной документации API рекомендуется использовать Swagger UI интерфейсы каждого сервиса.
