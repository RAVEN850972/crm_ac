# API Документация для CRM-системы по монтажу кондиционеров

## Содержание
1. [Общая информация](#общая-информация)
2. [Аутентификация](#аутентификация)
3. [Пользователи (Users)](#пользователи-users)
4. [Клиенты (Clients)](#клиенты-clients)
5. [Услуги (Services)](#услуги-services)
6. [Заказы (Orders)](#заказы-orders)
7. [Позиции заказа (Order Items)](#позиции-заказа-order-items)
8. [Финансы (Finance)](#финансы-finance)
9. [Выплаты зарплат (Salary Payments)](#выплаты-зарплат-salary-payments)
10. [Дашборд и статистика](#дашборд-и-статистика)
11. [Экспорт данных](#экспорт-данных)
12. [Модальные окна API](#модальные-окна-api)

## Общая информация

**Базовый URL**: `http://example.com/api/`

**Форматы ответов**: JSON

**Статус-коды ответов**:
- 200 OK - Запрос успешно выполнен
- 201 Created - Ресурс успешно создан
- 400 Bad Request - Ошибка в запросе
- 401 Unauthorized - Требуется аутентификация
- 403 Forbidden - Нет доступа
- 404 Not Found - Ресурс не найден
- 500 Internal Server Error - Ошибка сервера

## Аутентификация

### Вход в систему

**Запрос**: `POST /user_accounts/login/`

**Тело запроса**:
```json
{
  "username": "string",
  "password": "string"
}
```

**Успешный ответ** (200 OK):
```json
{
  "success": true,
  "redirect": "/",
  "user": {
    "id": "integer",
    "username": "string",
    "full_name": "string",
    "role": "string"
  }
}
```

**Ошибка** (400 Bad Request):
```json
{
  "success": false,
  "error": "Неверное имя пользователя или пароль"
}
```

### Выход из системы

**Запрос**: `GET /user_accounts/logout/`

**Успешный ответ**: Редирект на страницу логина

## Пользователи (Users)

### Получение списка пользователей

**Запрос**: `GET /api/users/`

**Параметры запроса**:
- `role` (опционально) - Фильтр по роли (owner, manager, installer)
- `search` (опционально) - Поиск по имени, фамилии, email или username
- `page` (опционально) - Номер страницы для пагинации
- `page_size` (опционально) - Количество записей на странице

**Успешный ответ** (200 OK):
```json
{
  "count": "integer",
  "next": "string or null",
  "previous": "string or null",
  "results": [
    {
      "id": "integer",
      "username": "string",
      "first_name": "string",
      "last_name": "string",
      "email": "string",
      "role": "string",
      "phone": "string"
    }
  ]
}
```

### Получение информации о конкретном пользователе

**Запрос**: `GET /api/users/{id}/`

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "username": "string",
  "first_name": "string",
  "last_name": "string",
  "email": "string",
  "role": "string",
  "phone": "string"
}
```

### Создание пользователя

**Запрос**: `POST /api/users/`

**Тело запроса**:
```json
{
  "username": "string",
  "password": "string",
  "first_name": "string",
  "last_name": "string",
  "email": "string",
  "role": "string",
  "phone": "string"
}
```

**Успешный ответ** (201 Created):
```json
{
  "id": "integer",
  "username": "string",
  "first_name": "string",
  "last_name": "string",
  "email": "string",
  "role": "string",
  "phone": "string"
}
```

### Обновление пользователя

**Запрос**: `PUT /api/users/{id}/`

**Тело запроса**:
```json
{
  "first_name": "string",
  "last_name": "string",
  "email": "string",
  "role": "string",
  "phone": "string"
}
```

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "username": "string",
  "first_name": "string",
  "last_name": "string",
  "email": "string",
  "role": "string",
  "phone": "string"
}
```

### Частичное обновление пользователя

**Запрос**: `PATCH /api/users/{id}/`

**Тело запроса**: Любой набор полей из модели User

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "username": "string",
  "first_name": "string",
  "last_name": "string",
  "email": "string",
  "role": "string",
  "phone": "string"
}
```

### Удаление пользователя

**Запрос**: `DELETE /api/users/{id}/`

**Успешный ответ** (204 No Content)

## Клиенты (Clients)

### Получение списка клиентов

**Запрос**: `GET /api/clients/`

**Параметры запроса**:
- `source` (опционально) - Фильтр по источнику (avito, vk, website, recommendations, other)
- `search` (опционально) - Поиск по имени, телефону или адресу
- `page` (опционально) - Номер страницы
- `page_size` (опционально) - Размер страницы

**Успешный ответ** (200 OK):
```json
{
  "count": "integer",
  "next": "string or null",
  "previous": "string or null",
  "results": [
    {
      "id": "integer",
      "name": "string",
      "address": "string",
      "phone": "string",
      "source": "string",
      "created_at": "datetime"
    }
  ]
}
```

### Получение информации о конкретном клиенте

**Запрос**: `GET /api/clients/{id}/`

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "name": "string",
  "address": "string",
  "phone": "string",
  "source": "string",
  "created_at": "datetime"
}
```

### Создание клиента

**Запрос**: `POST /api/clients/`

**Тело запроса**:
```json
{
  "name": "string",
  "address": "string",
  "phone": "string",
  "source": "string"
}
```

**Успешный ответ** (201 Created):
```json
{
  "id": "integer",
  "name": "string",
  "address": "string",
  "phone": "string",
  "source": "string",
  "created_at": "datetime"
}
```

### Обновление клиента

**Запрос**: `PUT /api/clients/{id}/`

**Тело запроса**:
```json
{
  "name": "string",
  "address": "string",
  "phone": "string",
  "source": "string"
}
```

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "name": "string",
  "address": "string",
  "phone": "string",
  "source": "string",
  "created_at": "datetime"
}
```

### Частичное обновление клиента

**Запрос**: `PATCH /api/clients/{id}/`

**Тело запроса**: Любой набор полей из модели Client

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "name": "string",
  "address": "string",
  "phone": "string",
  "source": "string",
  "created_at": "datetime"
}
```

### Удаление клиента

**Запрос**: `DELETE /api/clients/{id}/`

**Успешный ответ** (204 No Content)

### Статистика клиентов по источникам

**Запрос**: `GET /api/clients/stats/by-source/`

**Успешный ответ** (200 OK):
```json
{
  "sources": [
    {
      "source": "string",
      "source_display": "string",
      "count": "integer"
    }
  ]
}
```

### Статистика клиентов по месяцам

**Запрос**: `GET /api/clients/stats/by-month/`

**Успешный ответ** (200 OK):
```json
{
  "months": [
    {
      "month": "string (YYYY-MM)",
      "count": "integer"
    }
  ]
}
```

## Услуги (Services)

### Получение списка услуг

**Запрос**: `GET /api/services/`

**Параметры запроса**:
- `category` (опционально) - Фильтр по категории (conditioner, installation, dismantling, maintenance, additional)
- `search` (опционально) - Поиск по названию
- `page` (опционально) - Номер страницы
- `page_size` (опционально) - Размер страницы

**Успешный ответ** (200 OK):
```json
{
  "count": "integer",
  "next": "string or null",
  "previous": "string or null",
  "results": [
    {
      "id": "integer",
      "name": "string",
      "cost_price": "decimal",
      "selling_price": "decimal",
      "category": "string",
      "created_at": "datetime"
    }
  ]
}
```

### Получение информации о конкретной услуге

**Запрос**: `GET /api/services/{id}/`

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "name": "string",
  "cost_price": "decimal",
  "selling_price": "decimal",
  "category": "string",
  "created_at": "datetime"
}
```

### Создание услуги

**Запрос**: `POST /api/services/`

**Тело запроса**:
```json
{
  "name": "string",
  "cost_price": "decimal",
  "selling_price": "decimal",
  "category": "string"
}
```

**Успешный ответ** (201 Created):
```json
{
  "id": "integer",
  "name": "string",
  "cost_price": "decimal",
  "selling_price": "decimal",
  "category": "string",
  "created_at": "datetime"
}
```

### Обновление услуги

**Запрос**: `PUT /api/services/{id}/`

**Тело запроса**:
```json
{
  "name": "string",
  "cost_price": "decimal",
  "selling_price": "decimal",
  "category": "string"
}
```

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "name": "string",
  "cost_price": "decimal",
  "selling_price": "decimal",
  "category": "string",
  "created_at": "datetime"
}
```

### Частичное обновление услуги

**Запрос**: `PATCH /api/services/{id}/`

**Тело запроса**: Любой набор полей из модели Service

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "name": "string",
  "cost_price": "decimal",
  "selling_price": "decimal",
  "category": "string",
  "created_at": "datetime"
}
```

### Удаление услуги

**Запрос**: `DELETE /api/services/{id}/`

**Успешный ответ** (204 No Content)

### Статистика услуг по категориям

**Запрос**: `GET /api/services/stats/by-category/`

**Успешный ответ** (200 OK):
```json
{
  "categories": [
    {
      "category": "string",
      "category_display": "string",
      "count": "integer"
    }
  ]
}
```

### Самые популярные услуги

**Запрос**: `GET /api/services/stats/popular/`

**Успешный ответ** (200 OK):
```json
{
  "popular_services": [
    {
      "service_id": "integer",
      "service_name": "string",
      "category": "string",
      "category_display": "string",
      "count": "integer"
    }
  ]
}
```

## Заказы (Orders)

### Получение списка заказов

**Запрос**: `GET /api/orders/`

**Параметры запроса**:
- `status` (опционально) - Фильтр по статусу (new, in_progress, completed)
- `manager` (опционально) - Фильтр по ID менеджера
- `client` (опционально) - Фильтр по ID клиента
- `search` (опционально) - Поиск по имени клиента
- `page` (опционально) - Номер страницы
- `page_size` (опционально) - Размер страницы

**Успешный ответ** (200 OK):
```json
{
  "count": "integer",
  "next": "string or null",
  "previous": "string or null",
  "results": [
    {
      "id": "integer",
      "client": "integer",
      "manager": "integer",
      "status": "string",
      "installers": ["integer", "integer"],
      "total_cost": "decimal",
      "items": [
        {
          "id": "integer",
          "service": "integer",
          "price": "decimal",
          "seller": "integer",
          "created_at": "datetime"
        }
      ],
      "created_at": "datetime",
      "completed_at": "datetime or null"
    }
  ]
}
```

### Получение информации о конкретном заказе

**Запрос**: `GET /api/orders/{id}/`

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "client": "integer",
  "manager": "integer",
  "status": "string",
  "installers": ["integer", "integer"],
  "total_cost": "decimal",
  "items": [
    {
      "id": "integer",
      "service": "integer",
      "price": "decimal",
      "seller": "integer",
      "created_at": "datetime"
    }
  ],
  "created_at": "datetime",
  "completed_at": "datetime or null"
}
```

### Создание заказа

**Запрос**: `POST /api/orders/`

**Тело запроса**:
```json
{
  "client": "integer",
  "manager": "integer",
  "status": "string",
  "installers": ["integer", "integer"],
  "items": [
    {
      "service": "integer",
      "price": "decimal",
      "seller": "integer"
    }
  ]
}
```

**Успешный ответ** (201 Created):
```json
{
  "id": "integer",
  "client": "integer",
  "manager": "integer",
  "status": "string",
  "installers": ["integer", "integer"],
  "total_cost": "decimal",
  "items": [
    {
      "id": "integer",
      "service": "integer",
      "price": "decimal",
      "seller": "integer",
      "created_at": "datetime"
    }
  ],
  "created_at": "datetime",
  "completed_at": "datetime or null"
}
```

### Обновление заказа

**Запрос**: `PUT /api/orders/{id}/`

**Тело запроса**:
```json
{
  "client": "integer",
  "manager": "integer",
  "status": "string",
  "installers": ["integer", "integer"]
}
```

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "client": "integer",
  "manager": "integer",
  "status": "string",
  "installers": ["integer", "integer"],
  "total_cost": "decimal",
  "items": [
    {
      "id": "integer",
      "service": "integer",
      "price": "decimal",
      "seller": "integer",
      "created_at": "datetime"
    }
  ],
  "created_at": "datetime",
  "completed_at": "datetime or null"
}
```

### Частичное обновление заказа

**Запрос**: `PATCH /api/orders/{id}/`

**Тело запроса**: Любой набор полей из модели Order

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "client": "integer",
  "manager": "integer",
  "status": "string",
  "installers": ["integer", "integer"],
  "total_cost": "decimal",
  "items": [
    {
      "id": "integer",
      "service": "integer",
      "price": "decimal",
      "seller": "integer",
      "created_at": "datetime"
    }
  ],
  "created_at": "datetime",
  "completed_at": "datetime or null"
}
```

### Удаление заказа

**Запрос**: `DELETE /api/orders/{id}/`

**Успешный ответ** (204 No Content)

### Добавление позиции в заказ

**Запрос**: `POST /api/orders/{id}/add_item/`

**Тело запроса**:
```json
{
  "service": "integer",
  "price": "decimal",
  "seller": "integer"
}
```

**Успешный ответ** (200 OK): Полный объект заказа со всеми позициями

### Изменение статуса заказа

**Запрос**: `POST /api/orders/{id}/change_status/`

**Тело запроса**:
```json
{
  "status": "string"
}
```

**Успешный ответ** (200 OK): Полный объект заказа с обновленным статусом

### Статистика заказов по статусам

**Запрос**: `GET /api/orders/stats/by-status/`

**Успешный ответ** (200 OK):
```json
{
  "statuses": [
    {
      "status": "string",
      "status_display": "string",
      "count": "integer"
    }
  ]
}
```

### Статистика заказов по месяцам

**Запрос**: `GET /api/orders/stats/by-month/`

**Успешный ответ** (200 OK):
```json
{
  "months": [
    {
      "month": "string (YYYY-MM)",
      "count": "integer",
      "revenue": "decimal"
    }
  ]
}
```

### Статистика заказов по менеджерам

**Запрос**: `GET /api/orders/stats/by-manager/`

**Успешный ответ** (200 OK):
```json
{
  "managers": [
    {
      "manager_id": "integer",
      "manager_name": "string",
      "orders_count": "integer",
      "revenue": "decimal"
    }
  ]
}
```

## Позиции заказа (Order Items)

Позиции заказа управляются через API заказов. Отдельные эндпоинты для позиций заказа не предусмотрены. Для добавления позиции используйте `POST /api/orders/{id}/add_item/`.

## Финансы (Finance)

### Получение списка транзакций

**Запрос**: `GET /api/transactions/`

**Параметры запроса**:
- `type` (опционально) - Фильтр по типу (income, expense)
- `search` (опционально) - Поиск по описанию
- `page` (опционально) - Номер страницы
- `page_size` (опционально) - Размер страницы

**Успешный ответ** (200 OK):
```json
{
  "count": "integer",
  "next": "string or null",
  "previous": "string or null",
  "results": [
    {
      "id": "integer",
      "type": "string",
      "amount": "decimal",
      "description": "string",
      "order": "integer or null",
      "created_at": "datetime"
    }
  ]
}
```

### Получение информации о конкретной транзакции

**Запрос**: `GET /api/transactions/{id}/`

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "type": "string",
  "amount": "decimal",
  "description": "string",
  "order": "integer or null",
  "created_at": "datetime"
}
```

### Создание транзакции

**Запрос**: `POST /api/transactions/`

**Тело запроса**:
```json
{
  "type": "string",
  "amount": "decimal",
  "description": "string",
  "order": "integer or null"
}
```

**Успешный ответ** (201 Created):
```json
{
  "id": "integer",
  "type": "string",
  "amount": "decimal",
  "description": "string",
  "order": "integer or null",
  "created_at": "datetime"
}
```

### Обновление транзакции

**Запрос**: `PUT /api/transactions/{id}/`

**Тело запроса**:
```json
{
  "type": "string",
  "amount": "decimal",
  "description": "string",
  "order": "integer or null"
}
```

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "type": "string",
  "amount": "decimal",
  "description": "string",
  "order": "integer or null",
  "created_at": "datetime"
}
```

### Частичное обновление транзакции

**Запрос**: `PATCH /api/transactions/{id}/`

**Тело запроса**: Любой набор полей из модели Transaction

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "type": "string",
  "amount": "decimal",
  "description": "string",
  "order": "integer or null",
  "created_at": "datetime"
}
```

### Удаление транзакции

**Запрос**: `DELETE /api/transactions/{id}/`

**Успешный ответ** (204 No Content)

### Получение баланса компании и статистики доходов/расходов

**Запрос**: `GET /api/finance/balance/`

**Успешный ответ** (200 OK):
```json
{
  "balance": "decimal",
  "monthly_stats": [
    {
      "month": "string (YYYY-MM)",
      "income": "decimal",
      "expense": "decimal",
      "profit": "decimal"
    }
  ]
}
```

### Получение расширенной финансовой статистики

**Запрос**: `GET /api/finance/stats/`

**Успешный ответ** (200 OK):
```json
{
  "income_this_month": "decimal",
  "expense_this_month": "decimal",
  "profit_this_month": "decimal",
  "daily_stats": [
    {
      "date": "string (YYYY-MM-DD)",
      "income": "decimal",
      "expense": "decimal",
      "profit": "decimal"
    }
  ]
}
```

## Выплаты зарплат (Salary Payments)

### Получение списка выплат зарплат

**Запрос**: `GET /api/salary-payments/`

**Параметры запроса**:
- `user` (опционально) - Фильтр по ID пользователя
- `search` (опционально) - Поиск по имени пользователя
- `page` (опционально) - Номер страницы
- `page_size` (опционально) - Размер страницы

**Успешный ответ** (200 OK):
```json
{
  "count": "integer",
  "next": "string or null",
  "previous": "string or null",
  "results": [
    {
      "id": "integer",
      "user": "integer",
      "amount": "decimal",
      "period_start": "date",
      "period_end": "date",
      "created_at": "datetime"
    }
  ]
}
```

### Получение информации о конкретной выплате зарплаты

**Запрос**: `GET /api/salary-payments/{id}/`

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "user": "integer",
  "amount": "decimal",
  "period_start": "date",
  "period_end": "date",
  "created_at": "datetime"
}
```

### Создание выплаты зарплаты

**Запрос**: `POST /api/salary-payments/`

**Тело запроса**:
```json
{
  "user": "integer",
  "amount": "decimal",
  "period_start": "date (YYYY-MM-DD)",
  "period_end": "date (YYYY-MM-DD)"
}
```

**Успешный ответ** (201 Created):
```json
{
  "id": "integer",
  "user": "integer",
  "amount": "decimal",
  "period_start": "date",
  "period_end": "date",
  "created_at": "datetime"
}
```

### Расчет зарплаты для сотрудника

**Запрос**: `GET /api/finance/calculate-salary/{user_id}/`

**Параметры запроса**:
- `start_date` (опционально) - Начало периода в формате YYYY-MM-DD
- `end_date` (опционально) - Конец периода в формате YYYY-MM-DD

**Успешный ответ** (200 OK) для монтажника:
```json
{
  "salary": {
    "installation_pay": "decimal",
    "additional_pay": "decimal",
    "penalties": "decimal",
    "total_salary": "decimal",
    "completed_orders_count": "integer",
    "additional_services_count": "integer"
  }
}
```

**Успешный ответ** (200 OK) для менеджера:
```json
{
  "salary": {
    "fixed_salary": "decimal",
    "orders_pay": "decimal",
    "conditioner_pay": "decimal",
    "additional_pay": "decimal",
    "total_salary": "decimal",
    "completed_orders_count": "integer",
    "conditioner_sales_count": "integer",
    "additional_sales_count": "integer"
  }
}
```

**Успешный ответ** (200 OK) для владельца:
```json
{
  "salary": {
    "installation_pay": "decimal",
    "total_revenue": "decimal",
    "total_cost_price": "decimal",
    "installers_pay": "decimal",
    "managers_pay": "decimal",
    "remaining_profit": "decimal",
    "total_salary": "decimal",
    "completed_orders_count": "integer"
  }
}
```

## Дашборд и статистика

### Получение статистики для дашборда

**Запрос**: `GET /api/dashboard/stats/`

**Успешный ответ** (200 OK):
```json
{
  "total_orders": "integer",
  "completed_orders": "integer",
  "orders_this_month": "integer",
  "total_clients": "integer",
  "clients_this_month": "integer",
  "company_balance": "decimal",
  "income_this_month": "decimal",
  "expense_this_month": "decimal",
  "orders_by_month": [
    {
      "month": "string (YYYY-MM)",
      "count": "integer",
      "revenue": "decimal"
    }
  ],
  "top_managers": [
    {
      "id": "integer",
      "name": "string",
      "orders_count": "integer",
      "revenue": "decimal"
    }
  ],
  "recent_orders": [
    {
      // Объекты заказов
    }
  ]
}
```

## Экспорт данных

### Экспорт клиентов в Excel

**Запрос**: `GET /api/export/clients/`

**Успешный ответ**: Файл Excel с данными клиентов

### Экспорт заказов в Excel

**Запрос**: `GET /api/export/orders/`

**Успешный ответ**: Файл Excel с данными заказов

### Экспорт финансов в Excel

**Запрос**: `GET /api/export/finance/`

**Успешный ответ**: Файл Excel с данными финансовых операций

## Модальные окна API

Эти эндпоинты предназначены для работы с модальными окнами в интерфейсе.

### Модальное окно для клиента

#### Получение данных для создания/редактирования клиента

**Запрос**: `GET /api/modal/client/` (создание нового) или `GET /api/modal/client/{client_id}/` (редактирование)

**Успешный ответ** для создания (200 OK):
```json
{
  "sources": [
    {"value": "avito", "label": "Авито"},
    {"value": "vk", "label": "ВК"},
    {"value": "website", "label": "Сайт"},
    {"value": "recommendations", "label": "Рекомендации"},
    {"value": "other", "label": "Другое"}
  ]
}
```

**Успешный ответ** для редактирования (200 OK):
```json
{
  "id": "integer",
  "name": "string",
  "address": "string",
  "phone": "string",
  "source": "string",
  "created_at": "datetime"
}
```

#### Создание клиента через модальное окно

**Запрос**: `POST /api/modal/client/`

**Тело запроса**:
```json
{
  "name": "string",
  "address": "string",
  "phone": "string",
  "source": "string"
}
```

**Успешный ответ** (201 Created):
```json
{
  "id": "integer",
  "name": "string",
  "address": "string",
  "phone": "string",
  "source": "string",
  "created_at": "datetime"
}
```

#### Обновление клиента через модальное окно

**Запрос**: `PUT /api/modal/client/{client_id}/`

**Тело запроса**:
```json
{
  "name": "string",
  "address": "string",
  "phone": "string",
  "source": "string"
}
```

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "name": "string",
  "address": "string",
  "phone": "string",
  "source": "string",
  "created_at": "datetime"
}
```

### Модальное окно для заказа

#### Получение данных для создания/редактирования заказа

**Запрос**: `GET /api/modal/order/` (создание нового) или `GET /api/modal/order/{order_id}/` (редактирование)

**Успешный ответ** для создания (200 OK):
```json
{
  "clients": [
    {"id": "integer", "name": "string", "phone": "string"}
  ],
  "managers": [
    {"id": "integer", "first_name": "string", "last_name": "string"}
  ],
  "installers": [
    {"id": "integer", "first_name": "string", "last_name": "string"}
  ],
  "statuses": [
    {"value": "new", "label": "Новый"},
    {"value": "in_progress", "label": "В работе"},
    {"value": "completed", "label": "Завершен"}
  ]
}
```

**Успешный ответ** для редактирования (200 OK):
```json
{
  "clients": [
    {"id": "integer", "name": "string", "phone": "string"}
  ],
  "managers": [
    {"id": "integer", "first_name": "string", "last_name": "string"}
  ],
  "installers": [
    {"id": "integer", "first_name": "string", "last_name": "string"}
  ],
  "statuses": [
    {"value": "new", "label": "Новый"},
    {"value": "in_progress", "label": "В работе"},
    {"value": "completed", "label": "Завершен"}
  ],
  "order": {
    "id": "integer",
    "client": "integer",
    "manager": "integer",
    "status": "string",
    "installers": ["integer", "integer"],
    "total_cost": "decimal",
    "created_at": "datetime",
    "completed_at": "datetime or null"
  },
  "items": [
    {
      "id": "integer",
      "service": "integer",
      "price": "decimal",
      "seller": "integer",
      "created_at": "datetime"
    }
  ]
}
```

#### Создание заказа через модальное окно

**Запрос**: `POST /api/modal/order/`

**Тело запроса**:
```json
{
  "client": "integer",
  "manager": "integer",
  "status": "string",
  "installers": ["integer", "integer"],
  "items": [
    {
      "service": "integer",
      "price": "decimal",
      "seller": "integer"
    }
  ]
}
```

**Успешный ответ** (201 Created):
```json
{
  "id": "integer",
  "client": "integer",
  "manager": "integer",
  "status": "string",
  "installers": ["integer", "integer"],
  "total_cost": "decimal",
  "created_at": "datetime",
  "completed_at": "datetime or null"
}
```

#### Обновление заказа через модальное окно

**Запрос**: `PUT /api/modal/order/{order_id}/`

**Тело запроса**:
```json
{
  "client": "integer",
  "manager": "integer",
  "status": "string",
  "installers": ["integer", "integer"]
}
```

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "client": "integer",
  "manager": "integer",
  "status": "string",
  "installers": ["integer", "integer"],
  "total_cost": "decimal",
  "created_at": "datetime",
  "completed_at": "datetime or null"
}
```

### Модальное окно для позиций заказа

#### Получение данных для добавления позиции в заказ

**Запрос**: `GET /api/modal/order/{order_id}/items/`

**Успешный ответ** (200 OK):
```json
{
  "order": {
    "id": "integer",
    "client": "integer",
    "manager": "integer",
    "status": "string",
    "installers": ["integer", "integer"],
    "total_cost": "decimal",
    "created_at": "datetime",
    "completed_at": "datetime or null"
  },
  "services": [
    {"id": "integer", "name": "string", "category": "string", "selling_price": "decimal"}
  ],
  "sellers": [
    {"id": "integer", "first_name": "string", "last_name": "string", "role": "string"}
  ]
}
```

#### Добавление позиции в заказ через модальное окно

**Запрос**: `POST /api/modal/order/{order_id}/items/`

**Тело запроса**:
```json
{
  "service": "integer",
  "price": "decimal",
  "seller": "integer"
}
```

**Успешный ответ** (201 Created):
```json
{
  "item": {
    "id": "integer",
    "service": "integer",
    "price": "decimal",
    "seller": "integer",
    "created_at": "datetime"
  },
  "order": {
    "id": "integer",
    "client": "integer",
    "manager": "integer",
    "status": "string",
    "installers": ["integer", "integer"],
    "total_cost": "decimal",
    "created_at": "datetime",
    "completed_at": "datetime or null"
  }
}
```

#### Удаление позиции из заказа через модальное окно

**Запрос**: `DELETE /api/modal/order/{order_id}/items/{item_id}/`

**Успешный ответ** (200 OK):
```json
{
  "order": {
    "id": "integer",
    "client": "integer",
    "manager": "integer",
    "status": "string",
    "installers": ["integer", "integer"],
    "total_cost": "decimal",
    "created_at": "datetime",
    "completed_at": "datetime or null"
  },
  "message": "Позиция успешно удалена"
}
```

### Модальное окно для транзакций

#### Получение данных для создания/редактирования транзакции

**Запрос**: `GET /api/modal/transaction/` (создание нового) или `GET /api/modal/transaction/{transaction_id}/` (редактирование)

**Успешный ответ** для создания (200 OK):
```json
{
  "types": [
    {"value": "income", "label": "Доход"},
    {"value": "expense", "label": "Расход"}
  ],
  "orders": [
    {"id": "integer", "client__name": "string"}
  ]
}
```

**Успешный ответ** для редактирования (200 OK):
```json
{
  "types": [
    {"value": "income", "label": "Доход"},
    {"value": "expense", "label": "Расход"}
  ],
  "orders": [
    {"id": "integer", "client__name": "string"}
  ],
  "transaction": {
    "id": "integer",
    "type": "string",
    "amount": "decimal",
    "description": "string",
    "order": "integer or null",
    "created_at": "datetime"
  }
}
```

#### Создание транзакции через модальное окно

**Запрос**: `POST /api/modal/transaction/`

**Тело запроса**:
```json
{
  "type": "string",
  "amount": "decimal",
  "description": "string",
  "order": "integer or null"
}
```

**Успешный ответ** (201 Created):
```json
{
  "id": "integer",
  "type": "string",
  "amount": "decimal",
  "description": "string",
  "order": "integer or null",
  "created_at": "datetime"
}
```

#### Обновление транзакции через модальное окно

**Запрос**: `PUT /api/modal/transaction/{transaction_id}/`

**Тело запроса**:
```json
{
  "type": "string",
  "amount": "decimal",
  "description": "string",
  "order": "integer or null"
}
```

**Успешный ответ** (200 OK):
```json
{
  "id": "integer",
  "type": "string",
  "amount": "decimal",
  "description": "string",
  "order": "integer or null",
  "created_at": "datetime"
}
```

### Модальное окно для выплаты зарплаты

#### Получение данных для создания выплаты зарплаты

**Запрос**: `GET /api/modal/salary-payment/{user_id}/`

**Успешный ответ** (200 OK):
```json
{
  "user": {
    "id": "integer",
    "username": "string",
    "first_name": "string",
    "last_name": "string",
    "email": "string",
    "role": "string",
    "phone": "string"
  },
  "salary_data": {
    // Для монтажника
    "installation_pay": "decimal",
    "additional_pay": "decimal",
    "penalties": "decimal",
    "total_salary": "decimal",
    "completed_orders_count": "integer",
    "additional_services_count": "integer"

    // ИЛИ для менеджера
    "fixed_salary": "decimal",
    "orders_pay": "decimal",
    "conditioner_pay": "decimal",
    "additional_pay": "decimal",
    "total_salary": "decimal",
    "completed_orders_count": "integer",
    "conditioner_sales_count": "integer",
    "additional_sales_count": "integer"

    // ИЛИ для владельца
    "installation_pay": "decimal",
    "total_revenue": "decimal",
    "total_cost_price": "decimal",
    "installers_pay": "decimal",
    "managers_pay": "decimal",
    "remaining_profit": "decimal",
    "total_salary": "decimal",
    "completed_orders_count": "integer"
  }
}
```

#### Создание выплаты зарплаты через модальное окно

**Запрос**: `POST /api/modal/salary-payment/{user_id}/`

**Тело запроса**:
```json
{
  "amount": "decimal",
  "period_start": "date (YYYY-MM-DD)",
  "period_end": "date (YYYY-MM-DD)"
}
```

**Успешный ответ** (201 Created):
```json
{
  "id": "integer",
  "user": "integer",
  "amount": "decimal",
  "period_start": "date",
  "period_end": "date",
  "created_at": "datetime"
}
```

## Коды ответов

- 200 OK: Запрос выполнен успешно.
- 201 Created: Ресурс успешно создан.
- 204 No Content: Запрос выполнен успешно, но нет данных для возврата.
- 400 Bad Request: Запрос не может быть обработан из-за синтаксической ошибки или неверных параметров.
- 401 Unauthorized: Для доступа к запрашиваемому ресурсу требуется аутентификация.
- 403 Forbidden: У аутентифицированного пользователя нет доступа к запрашиваемому ресурсу.
- 404 Not Found: Запрашиваемый ресурс не найден.
- 500 Internal Server Error: Ошибка на стороне сервера.

## Обработка ошибок

В случае ошибки API возвращает объект JSON со следующей структурой:

```json
{
  "error": "string",
  "detail": "string",
  "status_code": "integer"
}
```

Пример ответа с ошибкой:

```json
{
  "error": "Validation Error",
  "detail": "Это поле обязательно.",
  "status_code": 400
}
```

## Пагинация

Многие эндпоинты, возвращающие списки данных, поддерживают пагинацию. Для них могут быть указаны следующие параметры запроса:

- `page`: Номер страницы (по умолчанию 1)
- `page_size`: Количество элементов на странице (по умолчанию 20, максимум 100)

Формат ответа с пагинацией:

```json
{
  "count": "integer",
  "next": "string or null",
  "previous": "string or null",
  "results": []
}
```

где:
- `count`: Общее количество элементов
- `next`: URL следующей страницы (или null, если это последняя страница)
- `previous`: URL предыдущей страницы (или null, если это первая страница)
- `results`: Массив элементов текущей страницы

## Фильтрация

Многие эндпоинты поддерживают фильтрацию данных. Для этого используются параметры запроса, соответствующие полям моделей. Например:

```
GET /api/clients/?source=avito
```

вернет только клиентов из источника "avito".

Поиск по текстовым полям осуществляется с помощью параметра `search`:

```
GET /api/clients/?search=Иванов
```

вернет клиентов, у которых имя, телефон или адрес содержит "Иванов".

## Сортировка

Многие эндпоинты поддерживают сортировку данных. Для этого используется параметр `ordering`:

```
GET /api/orders/?ordering=-created_at
```

вернет заказы, отсортированные по дате создания в обратном порядке.

Для сортировки по возрастанию укажите имя поля без префикса:

```
GET /api/orders/?ordering=created_at
```

Можно указать несколько полей для сортировки, разделив их запятыми:

```
GET /api/orders/?ordering=-status,created_at
```

сначала отсортирует по статусу в обратном порядке, затем по дате создания в прямом порядке.

## Заключение

Данная документация содержит описание всех доступных API-эндпоинтов CRM-системы для бизнеса по монтажу кондиционеров. Эта документация предназначена для фронтенд-разработчиков, которые будут взаимодействовать с API при разработке пользовательского интерфейса системы.