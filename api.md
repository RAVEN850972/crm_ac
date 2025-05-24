# CRM для кондиционеров - API Документация

## Содержание

1. [Аутентификация](#аутентификация)
2. [Пользователи](#пользователи)
3. [Клиенты](#клиенты)
4. [Услуги](#услуги)
5. [Заказы](#заказы)
6. [Финансы](#финансы)
7. [Календарь и расписание](#календарь-и-расписание)
8. [Статистика и аналитика](#статистика-и-аналитика)
9. [Экспорт данных](#экспорт-данных)
10. [Модальные окна](#модальные-окна)

## Базовая информация

- **Base URL**: `http://localhost:8000/api/`
- **Формат данных**: JSON
- **Аутентификация**: Session-based / JWT
- **Пагинация**: 20 элементов на страницу (настраивается)

---

## Аутентификация

### Вход в систему
```http
POST /user_accounts/login/
```

**Тело запроса:**
```json
{
  "username": "manager1",
  "password": "password123"
}
```

**Ответ (успех):**
```json
{
  "success": true,
  "redirect": "/",
  "user": {
    "id": 1,
    "username": "manager1",
    "full_name": "Иван Иванов",
    "role": "manager"
  }
}
```

**Ответ (ошибка):**
```json
{
  "success": false,
  "error": "Неверное имя пользователя или пароль"
}
```

### Выход из системы
```http
POST /user_accounts/logout/
```

---

## Пользователи

### Список пользователей
```http
GET /api/users/
```

**Параметры запроса:**
- `role` - фильтр по роли (owner, manager, installer)
- `search` - поиск по имени/email/username

**Ответ:**
```json
{
  "count": 10,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": 1,
      "username": "manager1",
      "first_name": "Иван",
      "last_name": "Иванов",
      "email": "ivan@example.com",
      "role": "manager",
      "phone": "+7900123456"
    }
  ]
}
```

### Создание пользователя
```http
POST /api/users/
```

**Тело запроса:**
```json
{
  "username": "new_user",
  "first_name": "Имя",
  "last_name": "Фамилия",
  "email": "user@example.com",
  "role": "installer",
  "phone": "+7900123456",
  "password": "securepassword"
}
```

### Обновление пользователя
```http
PUT /api/users/{id}/
PATCH /api/users/{id}/
```

### Удаление пользователя
```http
DELETE /api/users/{id}/
```

---

## Клиенты

### Список клиентов
```http
GET /api/clients/
```

**Параметры запроса:**
- `source` - фильтр по источнику (avito, vk, website, recommendations, other)
- `search` - поиск по имени/телефону/адресу

**Ответ:**
```json
{
  "count": 25,
  "results": [
    {
      "id": 1,
      "name": "Петр Петров",
      "address": "г. Москва, ул. Ленина, 10",
      "phone": "+7900123456",
      "source": "avito",
      "created_at": "2025-05-24T10:30:00Z"
    }
  ]
}
```

### Создание клиента
```http
POST /api/clients/
```

**Тело запроса:**
```json
{
  "name": "Новый Клиент",
  "address": "г. Москва, ул. Новая, 5",
  "phone": "+7900654321",
  "source": "website"
}
```

### Статистика клиентов по источникам
```http
GET /api/clients/stats/by-source/
```

**Ответ:**
```json
{
  "sources": [
    {
      "source": "avito",
      "source_display": "Авито",
      "count": 15
    },
    {
      "source": "website",
      "source_display": "Сайт",
      "count": 8
    }
  ]
}
```

### Статистика клиентов по месяцам
```http
GET /api/clients/stats/by-month/
```

**Ответ:**
```json
{
  "months": [
    {
      "month": "2025-04",
      "count": 12
    },
    {
      "month": "2025-05",
      "count": 18
    }
  ]
}
```

---

## Услуги

### Список услуг
```http
GET /api/services/
```

**Параметры запроса:**
- `category` - фильтр по категории (conditioner, installation, dismantling, maintenance, additional)
- `search` - поиск по названию

**Ответ:**
```json
{
  "results": [
    {
      "id": 1,
      "name": "Кондиционер LG 12000 BTU",
      "cost_price": "25000.00",
      "selling_price": "35000.00",
      "category": "conditioner",
      "category_display": "Кондиционер",
      "created_at": "2025-05-01T12:00:00Z"
    }
  ]
}
```

### Создание услуги
```http
POST /api/services/
```

**Тело запроса:**
```json
{
  "name": "Монтаж сплит-системы",
  "cost_price": "3000.00",
  "selling_price": "5000.00",
  "category": "installation"
}
```

### Статистика услуг по категориям
```http
GET /api/services/stats/by-category/
```

### Популярные услуги
```http
GET /api/services/stats/popular/
```

**Ответ:**
```json
{
  "popular_services": [
    {
      "service_id": 1,
      "service_name": "Монтаж кондиционера",
      "category": "installation",
      "category_display": "Монтаж",
      "count": 45
    }
  ]
}
```

---

## Заказы

### Список заказов
```http
GET /api/orders/
```

**Параметры запроса:**
- `status` - фильтр по статусу (new, in_progress, completed)
- `manager` - ID менеджера
- `client` - ID клиента
- `search` - поиск по имени клиента

**Ответ:**
```json
{
  "results": [
    {
      "id": 1,
      "client": 1,
      "client_name": "Петр Петров",
      "client_phone": "+7900123456",
      "client_address": "г. Москва, ул. Ленина, 10",
      "manager": 2,
      "manager_name": "Иван Иванов",
      "status": "new",
      "status_display": "Новый",
      "installers": [3, 4],
      "installers_names": [
        {"id": 3, "name": "Алексей Сидоров"},
        {"id": 4, "name": "Михаил Петров"}
      ],
      "total_cost": "45000.00",
      "created_at": "2025-05-24T10:00:00Z",
      "completed_at": null,
      "items": [
        {
          "id": 1,
          "service": 1,
          "service_name": "Кондиционер LG",
          "service_category": "conditioner",
          "price": "35000.00",
          "seller": 2,
          "seller_name": "Иван Иванов"
        }
      ]
    }
  ]
}
```

### Создание заказа
```http
POST /api/orders/
```

**Тело запроса:**
```json
{
  "client": 1,
  "manager": 2,
  "status": "new",
  "installers": [3, 4]
}
```

### Добавление позиции в заказ
```http
POST /api/orders/{order_id}/add_item/
```

**Тело запроса:**
```json
{
  "service": 1,
  "price": "35000.00",
  "seller": 2
}
```

### Изменение статуса заказа
```http
POST /api/orders/{order_id}/change_status/
```

**Тело запроса:**
```json
{
  "status": "completed"
}
```

### Статистика заказов по статусам
```http
GET /api/orders/stats/by-status/
```

### Статистика заказов по месяцам
```http
GET /api/orders/stats/by-month/
```

### Статистика заказов по менеджерам
```http
GET /api/orders/stats/by-manager/
```

**Ответ:**
```json
{
  "managers": [
    {
      "manager_id": 2,
      "manager_name": "Иван Иванов",
      "orders_count": 15,
      "revenue": 450000.00
    }
  ]
}
```

---

## Финансы

### Список транзакций
```http
GET /api/transactions/
```

**Параметры запроса:**
- `type` - фильтр по типу (income, expense)
- `search` - поиск по описанию

**Ответ:**
```json
{
  "results": [
    {
      "id": 1,
      "type": "income",
      "type_display": "Доход",
      "amount": "45000.00",
      "description": "Доход от завершения заказа #1",
      "order": 1,
      "order_display": "Заказ #1 - Петр Петров",
      "created_at": "2025-05-24T15:30:00Z"
    }
  ]
}
```

### Создание транзакции
```http
POST /api/transactions/
```

**Тело запроса:**
```json
{
  "type": "expense",
  "amount": "5000.00",
  "description": "Расходы на материалы",
  "order": null
}
```

### Баланс компании
```http
GET /api/finance/balance/
```

**Ответ:**
```json
{
  "balance": 125000.50,
  "monthly_stats": [
    {
      "month": "2025-04",
      "income": 150000.00,
      "expense": 45000.00,
      "profit": 105000.00
    }
  ]
}
```

### Расширенная финансовая статистика
```http
GET /api/finance/stats/
```

**Ответ:**
```json
{
  "income_this_month": 85000.00,
  "expense_this_month": 25000.00,
  "profit_this_month": 60000.00,
  "daily_stats": [
    {
      "date": "2025-05-23",
      "income": 15000.00,
      "expense": 3000.00,
      "profit": 12000.00
    }
  ]
}
```

### Расчет зарплаты
```http
GET /api/finance/calculate-salary/{user_id}/
```

**Параметры запроса:**
- `start_date` - начальная дата (YYYY-MM-DD)
- `end_date` - конечная дата (YYYY-MM-DD)

**Ответ для монтажника:**
```json
{
  "salary": {
    "installation_pay": 15000.00,
    "additional_pay": 2500.00,
    "penalties": 0.00,
    "total_salary": 17500.00,
    "completed_orders_count": 10,
    "additional_services_count": 5
  }
}
```

**Ответ для менеджера:**
```json
{
  "salary": {
    "fixed_salary": 30000.00,
    "orders_pay": 5000.00,
    "conditioner_pay": 8000.00,
    "additional_pay": 3000.00,
    "total_salary": 46000.00,
    "completed_orders_count": 20,
    "conditioner_sales_count": 8,
    "additional_sales_count": 6
  }
}
```

---

## Календарь и расписание

### Получение календаря
```http
GET /api/calendar/
```

**Параметры запроса:**
- `start_date` - начальная дата (YYYY-MM-DD)
- `end_date` - конечная дата (YYYY-MM-DD)
- `installer_id` - ID монтажника (опционально)

**Ответ:**
```json
{
  "calendar": {
    "2025-05-24": [
      {
        "id": 1,
        "order_id": 1,
        "client_name": "Петр Петров",
        "client_address": "г. Москва, ул. Ленина, 10",
        "client_phone": "+7900123456",
        "manager": "Иван Иванов",
        "start_time": "09:00",
        "end_time": "12:00",
        "status": "scheduled",
        "status_display": "Запланировано",
        "priority": "normal",
        "priority_display": "Обычный",
        "installers": [
          {"id": 3, "name": "Алексей Сидоров"}
        ],
        "notes": "Третий этаж, домофон 123",
        "is_overdue": false,
        "estimated_duration": "3:00:00"
      }
    ]
  },
  "total_schedules": 15
}
```

### Создание расписания
```http
POST /api/calendar/
```

**Тело запроса:**
```json
{
  "order_id": 1,
  "scheduled_date": "2025-05-25",
  "start_time": "10:00",
  "end_time": "13:00",
  "installer_ids": [3, 4],
  "priority": "high",
  "notes": "Срочный монтаж",
  "estimated_duration": "2:30:00"
}
```

### Детали расписания
```http
GET /api/calendar/schedule/{schedule_id}/
```

### Обновление расписания
```http
PUT /api/calendar/schedule/{schedule_id}/
PATCH /api/calendar/schedule/{schedule_id}/
```

### Удаление расписания
```http
DELETE /api/calendar/schedule/{schedule_id}/
```

### Начало работы
```http
POST /api/calendar/schedule/{schedule_id}/start/
```

**Ответ:**
```json
{
  "message": "Работа начата",
  "actual_start_time": "2025-05-24T09:15:00Z",
  "status": "in_progress"
}
```

### Завершение работы
```http
POST /api/calendar/schedule/{schedule_id}/complete/
```

**Ответ:**
```json
{
  "message": "Работа завершена",
  "actual_end_time": "2025-05-24T11:45:00Z",
  "duration": "2:30:00",
  "status": "completed"
}
```

### Проверка доступности монтажников
```http
POST /api/calendar/availability/check/
```

**Тело запроса:**
```json
{
  "installer_ids": [3, 4],
  "date": "2025-05-25",
  "start_time": "10:00",
  "end_time": "13:00"
}
```

**Ответ:**
```json
{
  "available": false,
  "conflicts": ["Алексей Сидоров"],
  "message": "Конфликты: Алексей Сидоров"
}
```

### Расписание конкретного монтажника
```http
GET /api/calendar/installer/{installer_id}/schedule/
```

**Параметры запроса:**
- `start_date` - начальная дата
- `end_date` - конечная дата

### Оптимизация маршрута
```http
GET /api/calendar/routes/
POST /api/calendar/routes/optimize/
```

**Параметры запроса (GET):**
- `installer_id` - ID монтажника
- `date` - дата (YYYY-MM-DD)

**Тело запроса (POST):**
```json
{
  "installer_id": 3,
  "date": "2025-05-25"
}
```

**Ответ:**
```json
{
  "route_id": 1,
  "installer": "Алексей Сидоров",
  "date": "2025-05-25",
  "total_distance": 45.7,
  "total_travel_time": "2:15:00",
  "is_optimized": true,
  "start_location": "Склад компании",
  "points": [
    {
      "sequence": 1,
      "arrival_time": "08:30",
      "departure_time": "11:00",
      "client_name": "Петр Петров",
      "client_address": "г. Москва, ул. Ленина, 10",
      "client_phone": "+7900123456",
      "order_id": 1,
      "status": "scheduled",
      "priority": "high",
      "travel_distance": 12.5,
      "travel_time": "0:25:00"
    }
  ]
}
```

---

## Статистика и аналитика

### Статистика дашборда
```http
GET /api/dashboard/stats/
```

**Ответ:**
```json
{
  "total_orders": 156,
  "completed_orders": 120,
  "orders_this_month": 25,
  "total_clients": 89,
  "clients_this_month": 12,
  "company_balance": 325000.50,
  "income_this_month": 125000.00,
  "expense_this_month": 45000.00,
  "orders_by_month": [
    {
      "month": "2025-04",
      "count": 18,
      "revenue": 185000.00
    }
  ],
  "top_managers": [
    {
      "id": 2,
      "name": "Иван Иванов",
      "orders_count": 25,
      "revenue": 275000.00
    }
  ],
  "recent_orders": [
    {
      "id": 156,
      "client_name": "Новый Клиент",
      "total_cost": "45000.00",
      "status": "new",
      "created_at": "2025-05-24T14:30:00Z"
    }
  ]
}
```

---

## Экспорт данных

### Экспорт клиентов
```http
GET /api/export/clients/
```

**Ответ:** Excel файл (.xlsx)

### Экспорт заказов
```http
GET /api/export/orders/
```

**Ответ:** Excel файл (.xlsx)

### Экспорт финансов
```http
GET /api/export/finance/
```

**Ответ:** Excel файл (.xlsx)

---

## Модальные окна

### Данные для модального окна клиента

#### Создание клиента
```http
GET /api/modal/client/
```

**Ответ:**
```json
{
  "sources": [
    {"value": "avito", "label": "Авито"},
    {"value": "vk", "label": "ВК"},
    {"value": "website", "label": "Сайт"}
  ]
}
```

#### Редактирование клиента
```http
GET /api/modal/client/{client_id}/
```

**Ответ:**
```json
{
  "id": 1,
  "name": "Петр Петров",
  "address": "г. Москва, ул. Ленина, 10",
  "phone": "+7900123456",
  "source": "avito",
  "created_at": "2025-05-24T10:30:00Z"
}
```

#### Создание/обновление клиента
```http
POST /api/modal/client/
PUT /api/modal/client/{client_id}/
```

### Данные для модального окна заказа

#### Создание заказа
```http
GET /api/modal/order/
```

**Ответ:**
```json
{
  "clients": [
    {"id": 1, "name": "Петр Петров", "phone": "+7900123456"}
  ],
  "managers": [
    {"id": 2, "first_name": "Иван", "last_name": "Иванов"}
  ],
  "installers": [
    {"id": 3, "first_name": "Алексей", "last_name": "Сидоров"}
  ],
  "statuses": [
    {"value": "new", "label": "Новый"},
    {"value": "in_progress", "label": "В работе"}
  ]
}
```

#### Редактирование заказа
```http
GET /api/modal/order/{order_id}/
```

### Добавление позиции в заказ
```http
GET /api/modal/order/{order_id}/items/
```

**Ответ:**
```json
{
  "order": {
    "id": 1,
    "client_name": "Петр Петров",
    "total_cost": "35000.00"
  },
  "services": [
    {
      "id": 1,
      "name": "Кондиционер LG",
      "category": "conditioner",
      "category_display": "Кондиционер",
      "selling_price": "35000.00"
    }
  ],
  "sellers": [
    {"id": 2, "first_name": "Иван", "last_name": "Иванов", "role": "manager"}
  ]
}
```

#### Создание позиции заказа
```http
POST /api/modal/order/{order_id}/items/
```

**Тело запроса:**
```json
{
  "service": 1,
  "price": "35000.00",
  "seller": 2
}
```

#### Удаление позиции заказа
```http
DELETE /api/modal/order/{order_id}/items/{item_id}/
```

### Данные для модального окна транзакции
```http
GET /api/modal/transaction/
GET /api/modal/transaction/{transaction_id}/
POST /api/modal/transaction/
PUT /api/modal/transaction/{transaction_id}/
```

### Данные для модального окна выплаты зарплаты
```http
GET /api/modal/salary-payment/{user_id}/
POST /api/modal/salary-payment/{user_id}/
```

---

## Коды ошибок

### HTTP статус коды
- `200` - Успешно
- `201` - Создано
- `204` - Удалено
- `400` - Ошибка валидации
- `401` - Не авторизован
- `403` - Доступ запрещен
- `404` - Не найдено
- `500` - Внутренняя ошибка сервера

### Типичные ошибки

**Ошибка валидации:**
```json
{
  "field_name": ["Это поле обязательно."],
  "another_field": ["Введите корректное значение."]
}
```

**Ошибка доступа:**
```json
{
  "error": "У вас нет прав для выполнения этого действия"
}
```

**Ошибка не найдено:**
```json
{
  "detail": "Не найдено."
}
```

---

## Права доступа по ролям

### Владелец (owner)
- Полный доступ ко всем endpoint'ам
- Создание, редактирование, удаление всех сущностей
- Просмотр финансовой информации
- Расчет и выплата зарплат

### Менеджер (manager)
- Управление своими заказами и клиентами
- Создание и планирование монтажей
- Просмотр своей статистики
- Добавление позиций в заказы

### Монтажник (installer)
- Просмотр своего расписания
- Отметка начала/завершения работ
- Просмотр заказов, где назначен
- Ограниченный доступ к изменению статусов

---

## Пагинация

Все списковые endpoint'ы поддерживают пагинацию:

**Параметры запроса:**
- `page` - номер страницы (по умолчанию 1)
- `page_size` - размер страницы (по умолчанию 20, максимум 100)

**Формат ответа:**
```json
{
  "count": 150,
  "next": "http://localhost:8000/api/orders/?page=2",
  "previous": null,
  "results": [...]
}
```

---

## Фильтрация и поиск

Большинство endpoint'ов поддерживают фильтрацию и поиск:

**Фильтрация:**
- По полям модели: `?status=new&role=manager`
- По связанным объектам: `?client__source=avito`

**Поиск:**
- `?search=текст` - поиск по указанным полям

**Сортировка:**
- `?ordering=created_at` - по возрастанию
- `?ordering=-created_at` - по убыванию

---

## Примеры использования

### Создание заказа с позициями
```javascript
// 1. Создаем заказ
const orderResponse = await fetch('/api/orders/', {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({
    client: 1,
    manager: 2,
    installers: [3]
  })
});
const order = await orderResponse.json();

// 2. Добавляем позиции
await fetch(`/api/orders/${order.id}/add_item/`, {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({
    service: 1,
    price: "35000.00",
    seller: 2
  })
});
```

### Планирование монтажа
```javascript
// 1. Проверяем доступность
const availabilityResponse = await fetch('/api/calendar/availability/check/', {
  method: 'POST',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({
    installer_ids: [3],
    date: "2025-05-25",
    start_time: "10:00",
    end_time: "13:00"
  })
});

// 2. Создаем расписание
if (availability.available) {
  await fetch('/api/calendar/', {
    method: 'POST',
    headers: {'Content-Type': 'application/json'},
    body: JSON.stringify({
      order_id: 1,
      scheduled_date: "2025-05-25",
      start_time: "10:00",
      end_time: "13:00",
      installer_ids: [3],
      priority: "normal"
    })
  });
}
```

### Получение статистики для дашборда
```javascript
const [dashboardStats, financeStats] = await Promise.all([
  fetch('/api/dashboard/stats/').then(r => r.json()),
  fetch('/api/finance/stats/').then(r => r.json())
]);
```

Эта документация покрывает все основные API endpoint'ы системы и должна быть достаточной для создания полнофункционального фронтенда.