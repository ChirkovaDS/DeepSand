# Проектирование REST API  

## Проектные решения  

1. **Архитектурный стиль**: RESTful API  
2. **Методы HTTP**: GET, POST, PUT, DELETE.  
3. **Идентификация пользователей** через токены.  
4. **Формат данных**: Формат JSON.  
5. **Обработка ошибок**: Реализована единая структура ошибок с кодами и описанием.  
6. **Структура URL**: 'api/{версия}/{модели данных}'. Cтруктуры: rawmaterials, plans, strategies, clients, ordes, sandtypes
7. **Статус-коды HTTP**: Использование стандартных кодов для ответа сервера (200, 500).  
8. **Версионирование**: см пункт 6 

---

## Список требуемых эндпоинтов API  

1. **Получение списка сырья**
2. **Получение конкретного сырья**
3. **Добавление сырья**
4. **Изменение конкретного сырья**
5. **Удаление конкретного сырья**
6. **Получение списка планов**
7. **Получение конкретного плана**
8.  **Добавление плана**
9.  **Изменение конкретного плана**
10. **Удаление конкретного плана**
11. **Получение списка стратегий**
12. **Получение списка стратегий по id менеджера**
13. **Получение конкретной стратегии**
14. **Добавление стратегии**
15. **Изменение конкретной стратегии**
16. **Удаление конкретной стратегии**
17. **Получение списка клиентов**
18. **Получение конкретного клиента**
19. **Добавление клиента**
20. **Изменение конкретного клиента**
21. **Удаление конкретного клиента**
22. **Получение списка всех заказов**
24. **Получение списка заказов по id клиента**
25. **Получение конкретного заказа**
26. **Добавление заказа**
27. **Изменение конкретного заказа**
28. **Удаление конкретного заказа**
30. **Получение списка сырья по id плана**
34. **Получение списка номенклатур по id сырья**
36. **Добавление списка номенклатур по id сырья**
37. **Изменение списка номенклатур по id сырья**
38. **Удаление списка номенклатур по id сырья**
39. **Получение списка заказов по id стратегии**

## Документация API  

Метод для получение списка сырья

GET  `/api/v1/rawmaterials`  
Parameters: не требуется
Body: Не требуется  
**Пример успешного ответа (200):**  
```json
[
  {
    "id": 1,
    "name": "DERT-126k",
    "place": "Пермский нерудный карьер",
    "amount": 1000,
    "price": 2000,
    "priority": 10
  },
  {
    "id": 2,
    "name": "DERT-126k",
    "place": "Нерудный карьер г. Березники",
    "amount": 700,
    "price": 2500,
    "priority": 4
  }
]
```
---

Метод для получение конкретного сырья  
GET  `/api/v1/rawmaterials/{id}`  
Parameters: `id` (целое число, идентификатор сырья)  
Body: Не требуется  
**Пример успешного ответа (200):**  
```json
{
  "id": 1,
  "name": "DERT-126k",
  "place": "Пермский нерудный карьер",
  "amount": 1000,
  "price": 2000,
  "priority": 10
}
```

---

Метод для добавления сырья  
POST  `/api/v1/rawmaterials`  
Parameters: Не требуется  
**Body:**  
```json
{
  "name": "SAYT1012",
  "place": "Карьер в Новосибирске",
  "amount": 500,
  "price": 2200,
  "priority": 8
}
```
**Пример успешного ответа (201):**  
```json
{
  "id": 3,
  "name": "SAYT1012",
  "place": "Карьер в Новосибирске",
  "amount": 500,
  "price": 2200,
  "priority": 8
}
```

---

Метод для изменения конкретного сырья  
PUT `/api/v1/rawmaterials/{id}`  
Parameters: `id` (целое число, идентификатор сырья)  
Body:
```json
{
  "name": "DERT-126k",
  "place": "Пермский нерудный карьер",
  "amount": 1200,
  "price": 2100,
  "priority": 9
}
```
**Пример успешного ответа (200):**  
```json
{
  "id": 1,
  "name": "DERT-126k",
  "place": "Пермский нерудный карьер",
  "amount": 1200,
  "price": 2100,
  "priority": 9
}
```

---

Метод для удаления конкретного сырья**  
DELETE   `/api/v1/rawmaterials/{id}`  
Parameters:`id` (целое число, идентификатор сырья)  
Body: Не требуется  
**Пример успешного ответа (204):**  
Ответ без тела (No Content).

---
Ниже приведены примеры эндпоинтов для методов, связанных с планами:

---

**Метод для получения списка планов**  
GET  `/api/v1/plans`  
**Parameters:** Не требуются  
**Body:** Не требуется  
**Пример успешного ответа (200):**  
```json
[
  {
    "id": 1,
    "createdDate": "2023-09-01T12:00:00Z",
    "startDate": "2023-09-10T08:00:00Z",
    "endDate": "2023-09-20T18:00:00Z",
    "strategyId": 5,
    "rawMaterials": [
      { "id": 1, "usageAmount": 100 },
      { "id": 2, "usageAmount": 200 }
    ]
  },
  {
    "id": 2,
    "createdDate": "2023-09-05T10:00:00Z",
    "startDate": "2023-09-15T08:00:00Z",
    "endDate": "2023-09-25T18:00:00Z",
    "strategyId": 0,
    "rawMaterials": [
      { "id": 1, "usageAmount": 50 }
    ]
  }
]
```

---

**Метод для получения конкретного плана**  
GET  `/api/v1/plans/{id}`  
**Parameters:**  `id` (целое число, идентификатор плана)  
**Body:** Не требуется  
**Пример успешного ответа (200):**  
```json
{
  "id": 1,
  "createdDate": "2023-09-01T12:00:00Z",
  "startDate": "2023-09-10T08:00:00Z",
  "endDate": "2023-09-20T18:00:00Z",
  "strategyId": 5,
  "rawMaterials": [
    { "id": 1, "usageAmount": 100 },
    { "id": 2, "usageAmount": 200 }
  ]
}
```

---

**Метод для добавления плана**  
POST   `/api/v1/plans`  
**Parameters:** Не требуются  
**Body:**  
```json
{
  "createdDate": "2023-09-01T12:00:00Z",
  "startDate": "2023-09-10T08:00:00Z",
  "endDate": "2023-09-20T18:00:00Z",
  "strategyId": 5,
  "rawMaterials": [
    { "id": 1, "usageAmount": 100 },
    { "id": 2, "usageAmount": 200 }
  ]
}
```
**Пример успешного ответа (201):**  
```json
{
  "id": 1,
  "createdDate": "2023-09-01T12:00:00Z",
  "startDate": "2023-09-10T08:00:00Z",
  "endDate": "2023-09-20T18:00:00Z",
  "strategyId": 5,
  "rawMaterials": [
    { "id": 1, "usageAmount": 100 },
    { "id": 2, "usageAmount": 200 }
  ]
}
```

---

**Метод для изменения конкретного плана**  
PUT  `/api/v1/plans/{id}`  
**Parameters:** `id` (целое число, идентификатор плана)  
**Body:**  
```json
{
  "createdDate": "2023-09-01T12:00:00Z",
  "startDate": "2023-09-11T08:00:00Z",
  "endDate": "2023-09-21T18:00:00Z",
  "strategyId": 5,
  "rawMaterials": [
    { "id": 1, "usageAmount": 150 },
    { "id": 2, "usageAmount": 250 }
  ]
}
```
**Пример успешного ответа (200):**  
```json
{
  "id": 1,
  "createdDate": "2023-09-01T12:00:00Z",
  "startDate": "2023-09-11T08:00:00Z",
  "endDate": "2023-09-21T18:00:00Z",
  "strategyId": 5,
  "rawMaterials": [
    { "id": 1, "usageAmount": 150 },
    { "id": 2, "usageAmount": 250 }
  ]
}
```

---

**Метод для удаления конкретного плана**  
DELETE   `/api/v1/plans/{id}`  
**Parameters:**  `id` (целое число, идентификатор плана)  
**Body:** Не требуется  
**Пример успешного ответа (204):**  
Ответ без тела (No Content).

---

Метод для получения списка стратегий
GET  `/api/v1/strategies`  
Parameters: Не требуются  
Body: Не требуется  
**Пример успешного ответа (200):**  
```json
[
  {
    "id": 1,
    "executionDate": "2023-09-30T00:00:00Z",
    "orderIds": [1, 2, 3],
    "revenue": 150000,
    "status": "active",
    "comment": "Первичная стратегия",
    "managerId": 10
  },
  {
    "id": 2,
    "executionDate": "2023-10-15T00:00:00Z",
    "orderIds": [4, 5],
    "revenue": 90000,
    "status": "completed",
    "comment": "Выполненная стратегия",
    "managerId": 11
  }
]
```

---

Метод для получения стратегий конкретного менеджера
GET  `/api/v1/strategies?managerId={managerId}`  
Parameters:`managerId` (целое число, идентификатор менеджера)  
Body: Не требуется  
**Пример успешного ответа (200):**  
```json
[
  {
    "id": 1,
    "executionDate": "2023-09-30T00:00:00Z",
    "orderIds": [1, 2, 3],
    "revenue": 150000,
    "status": "active",
    "comment": "Первичная стратегия",
    "managerId": 10
  }
]
```

---

Метод для получения конкретной стратегии
GET  `/api/v1/strategies/{id}`  
Parameters: `id` (целое число, идентификатор стратегии)  
Body: Не требуется  
```json
{
  "id": 1,
  "executionDate": "2023-09-30T00:00:00Z",
  "orderIds": [1, 2, 3],
  "revenue": 150000,
  "status": "active",
  "comment": "Первичная стратегия",
  "managerId": 10
}
```

---

Метод для добавления стратегии
POST  `/api/v1/strategies`  
Parameters: Не требуются  
Body:
```json
{
  "executionDate": "2023-09-30T00:00:00Z",
  "orderIds": [1, 2, 3],
  "revenue": 150000,
  "status": "active",
  "comment": "Первичная стратегия",
  "managerId": 10
}
```
**Пример успешного ответа (201):**  
```json
{
  "id": 3,
  "executionDate": "2023-09-30T00:00:00Z",
  "orderIds": [1, 2, 3],
  "revenue": 150000,
  "status": "active",
  "comment": "Первичная стратегия",
  "managerId": 10
}
```

---

**Метод для изменения конкретной стратегии**  
PUT  `/api/v1/strategies/{id}`  
**Parameters:** `id` (целое число, идентификатор стратегии)  
**Body:**  
```json
{
  "executionDate": "2023-10-05T00:00:00Z",
  "orderIds": [1, 2, 3, 4],
  "revenue": 160000,
  "status": "active",
  "comment": "Обновленная стратегия",
  "managerId": 10
}
```
**Пример успешного ответа (200):**  
```json
{
  "id": 1,
  "executionDate": "2023-10-05T00:00:00Z",
  "orderIds": [1, 2, 3, 4],
  "revenue": 160000,
  "status": "active",
  "comment": "Обновленная стратегия",
  "managerId": 10
}
```

---

**Метод для удаления конкретной стратегии**  
DELETE  `/api/v1/strategies/{id}`  
**Parameters:**  `id` (целое число, идентификатор стратегии)  
**Body:** Не требуется  
**Пример успешного ответа (204):**  
Ответ без тела (No Content).

---


**Метод для получения списка клиентов**  
GET `/api/v1/clients`  
**Parameters:** Не требуются  
**Body:** Не требуется  
**Пример успешного ответа (200):**  
```json
[
  {
    "id": 1,
    "name": "ООО ААА",
    "contactPerson": "Иван Иванов",
    "phone": "+7-123-456-78-90"
  },
  {
    "id": 2,
    "name": "ЗАО Пример",
    "contactPerson": "Петр Петров",
    "phone": "+7-987-654-32-10"
  }
]
```

---

**Метод для получения конкретного клиента**  
GET  `/api/v1/clients/{id}`  
**Parameters:**  `id` (целое число, идентификатор клиента)  
**Body:** Не требуется  
**Пример успешного ответа (200):**  
```json
{
  "id": 1,
  "name": "ООО Рога и Копыта",
  "contactPerson": "Иван Иванов",
  "phone": "+7-123-456-78-90"
}
```

---

**Метод для добавления клиента**  
POST  `/api/v1/clients`  
**Parameters:** Не требуются  
**Body:**  
```json
{
  "name": "ООО Новые Технологии",
  "contactPerson": "Сергей Сергеев",
  "phone": "+7-321-654-98-76"
}
```
**Пример успешного ответа (201):**  
```json
{
  "id": 3,
  "name": "ООО Новые Технологии",
  "contactPerson": "Сергей Сергеев",
  "phone": "+7-321-654-98-76"
}
```

---


**Метод для изменения конкретного клиента**  
PUT  `/api/v1/clients/{id}`  
**Parameters:** `id` (целое число, идентификатор клиента)  
**Body:**  
```json
{
  "name": "ООО Рога и Копыта",
  "contactPerson": "Иван Иванов",
  "phone": "+7-123-000-00-00"
}
```
**Пример успешного ответа (200):**  
```json
{
  "id": 1,
  "name": "ООО Рога и Копыта",
  "contactPerson": "Иван Иванов",
  "phone": "+7-123-000-00-00"
}
```

---

**Метод для удаления конкретного клиента**  
DELETE  `/api/v1/clients/{id}`  
**Parameters:** `id` (целое число, идентификатор клиента)  
**Body:** Не требуется  
**Пример успешного ответа (204):**  
Ответ без тела (No Content).

---

**Метод для получения списка всех заказов**  
GET  `/api/v1/orders`  
**Parameters:** Не требуются  
**Body:** Не требуется  
**Пример успешного ответа (200):**  
```json
[
  {
    "id": 1,
    "clientId": 1,
    "orderDate": "2023-09-01T12:00:00Z",
    "executionDate": "2023-09-05T12:00:00Z",
    "supplyVolume": 500,
    "upperFraction": 8,
    "lowerFraction": 4,
    "orderAmount": 10000,
    "status": "В ожидание",
    "comment": ""
  },
  {
    "id": 2,
    "clientId": 2,
    "orderDate": "2023-09-02T12:00:00Z",
    "executionDate": "2023-09-06T12:00:00Z",
    "supplyVolume": 300,
    "upperFraction": 20,
    "lowerFraction": 5,
    "orderAmount": 60000,
    "status": "Завершен",
    "comment": "Первый приориет"
  }
]
```

---

**Метод для получения списка заказов для конкретного клиента**  
GET  `/api/v1/orders?clientId={clientId}`  
**Parameters: `clientId` (целое число, идентификатор клиента)  
**Body:** Не требуется  
**Пример успешного ответа (200):**  
```json
[
  {
    "id": 1,
    "clientId": 1,
    "orderDate": "2023-09-01T12:00:00Z",
    "executionDate": "2023-09-05T12:00:00Z",
    "supplyVolume": 500,
    "upperFraction": 8,
    "lowerFraction": 4,
    "orderAmount": 10000,
    "status": "В ожидание",
    "comment": ""
  }
]
```

---

**Метод для получения конкретного заказа**  
GET `/api/v1/orders/{id}`  
**Parameters:** `id` (целое число, идентификатор заказа)  
**Body:** Не требуется  
**Пример успешного ответа (200):**  
```json
{
    "id": 1,
    "clientId": 1,
    "orderDate": "2023-09-01T12:00:00Z",
    "executionDate": "2023-09-05T12:00:00Z",
    "supplyVolume": 500,
    "upperFraction": 8,
    "lowerFraction": 4,
    "orderAmount": 10000,
    "status": "В ожидание",
    "comment": ""
}
```

---

**Метод для добавления заказа**  
POST  `/api/v1/orders`  
**Parameters:** Не требуются  
**Body:** 
```json
{
  "clientId": 1,
  "orderDate": "2023-09-01T12:00:00Z",
  "executionDate": "2023-09-05T12:00:00Z",
  "supplyVolume": 500,
  "upperFraction": 15,
  "lowerFraction": 5,
  "orderAmount": 100000,
  "status": "В ожидание",
  "comment": "Второй приоритете"
}
```
**Пример успешного ответа (201):**  
```json
{
  "id": 1,
  "clientId": 1,
  "orderDate": "2023-09-01T12:00:00Z",
  "executionDate": "2023-09-05T12:00:00Z",
  "supplyVolume": 500,
  "upperFraction": 80,
  "lowerFraction": 40,
  "orderAmount": 100000,
  "status": "В ожидание",
  "comment": "Второй приоритете"
}
```

---

**Метод для изменения конкретного заказа**  
PUT  `/api/v1/orders/{id}`  
**Parameters:**  `id` (целое число, идентификатор заказа)  
**Body:**  
```json
{
  "clientId": 1,
  "orderDate": "2023-09-01T12:00:00Z",
  "executionDate": "2023-09-06T12:00:00Z",
  "supplyVolume": 550,
  "upperFraction": 4,
  "lowerFraction": 2,
  "orderAmount": 110000,
  "status": "В ожидание",
  "comment": "Второй приоритете"
}
```
**Пример успешного ответа (200):**  
```json
{
  "id": 1,
  "clientId": 1,
  "orderDate": "2023-09-01T12:00:00Z",
  "executionDate": "2023-09-06T12:00:00Z",
  "supplyVolume": 550,
  "upperFraction": 82,
  "lowerFraction": 42,
  "orderAmount": 110000,
  "status": "В ожидание",
  "comment": "Второй приоритете"
}
```

---

**Метод для удаления конкретного заказа**  
DELETE   `/api/v1/orders/{id}`  
**Parameters:**  
- **Path:** `id` (целое число, идентификатор заказа)  
**Body:** Не требуется  
**Пример успешного ответа (204):**  
Ответ без тела (No Content).

---


> **Примечание:** Следующие эндпоинты работают с коллекцией типов песков, ассоциированной с конкретным сырьём.

**Метод для получения списка типов песков для заданного сырья**  
GET `/api/v1/sandtypes?rawmaterialId={rawMaterialId}`  
**Parameters:**  `rawMaterialId` (целое число, идентификатор сырья)  
**Body:** Не требуется  
**Пример успешного ответа (200):**  
```json
[
  {
    "id": 1,
    "rawMaterialId": 10,
    "upperFraction": 3,
    "sandContent": 350
  },
  {
    "id": 2,
    "rawMaterialId": 10,
    "upperFraction": 7,
    "sandContent": 520
  }
]
```

---

**Метод для добавления нового типа песков для заданного сырья**  
POST   `/api/v1/sandtypes?rawmaterialId={rawMaterialId}`  
**Parameters:**  `rawMaterialId` (целое число, идентификатор сырья)  
**Body:**  
```json
{
  "upperFraction": 10,
  "sandContent": 930
}
```
**Пример успешного ответа (201):**  
```json
{
  "id": 3,
  "rawMaterialId": 10,
  "upperFraction": 11,
  "sandContent": 930
}
```

---

**Метод для изменения существующего типа песков для заданного сырья**  
PUT  `/api/v1/sandtypes?rawmaterialId={rawMaterialId}`  
**Parameters:**  `rawMaterialId` (целое число, идентификатор сырья)  
**Body:**  
```json
{
  "id": 2,
  "upperFraction": 87,
  "sandContent": 325
}
```
**Пример успешного ответа (200):**  
```json
{
  "id": 2,
  "rawMaterialId": 10,
  "upperFraction": 87,
  "sandContent": 325
}
```

---

**Метод для удаления типа песков для заданного сырья**  
DELETE  `/api/v1/sandtypes?rawmaterialId={rawMaterialId}&typeId={typeId}`  
**Parameters:**  `rawMaterialId` (целое число, идентификатор сырья) и `typeId` (целое число, идентификатор типа песков)  
**Body:** Не требуется  
**Пример успешного ответа (204):**  
Ответ без тела (No Content).


## Реализация API  

Для реализации использовался ASP .Net Core Web API.

**Пример кода реализации**:  
```csharp
using Microsoft.AspNetCore.Mvc;
using System;
using System.Collections.Generic;
using System.Linq;

namespace DipSandApi
{
    // ============================
    // МОДЕЛИ
    // ============================

    public class RawMaterial
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Place { get; set; }
        public int Amount { get; set; }
        public decimal Price { get; set; }
        public int Priority { get; set; }
    }

    public class Plan
    {
        public int Id { get; set; }
        public DateTime CreatedDate { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime EndDate { get; set; }
        public int StrategyId { get; set; }
        // Список сырья, использованного в плане с указанием объёма использования
        public List<PlanRawMaterial> RawMaterials { get; set; }
    }

    public class PlanRawMaterial
    {
        public int Id { get; set; }
        public int UsageAmount { get; set; }
    }

    public class Strategy
    {
        public int Id { get; set; }
        public DateTime ExecutionDate { get; set; }
        public List<int> OrderIds { get; set; }
        public decimal Revenue { get; set; }
        public string Status { get; set; }
        public string Comment { get; set; }
        public int ManagerId { get; set; }
    }

    public class Client
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string ContactPerson { get; set; }
        public string Phone { get; set; }
        public int ManagerId { get; set; }
    }

    public class Order
    {
        public int Id { get; set; }
        public int ClientId { get; set; }
        public DateTime OrderDate { get; set; }
        public DateTime ExecutionDate { get; set; }
        public int SupplyVolume { get; set; }
        public int UpperFraction { get; set; }
        public int LowerFraction { get; set; }
        public decimal OrderAmount { get; set; }
        public string Status { get; set; }
        public string Comment { get; set; }
    }

    public class SandType
    {
        public int Id { get; set; }
        public int RawMaterialId { get; set; }
        public int UpperFraction { get; set; }
        public int SandContent { get; set; }
    }

    // ============================
    // КОНТРОЛЛЕР rawmaterials (сырье) - Эндпоинты 1-5
    // ============================

    [ApiController]
    [Route("api/v1/rawmaterials")]
    public class RawMaterialsController : ControllerBase
    {
        private static List<RawMaterial> RawMaterials = new List<RawMaterial>
        {
            new RawMaterial { Id = 1, Name = "DERT-126k", Place = "Пермский карьер", Amount = 1000, Price = 2000, Priority = 10 },
            new RawMaterial { Id = 2, Name = "DERT-126k", Place = "Березники карьер", Amount = 700, Price = 2500, Priority = 4 }
        };

        // 1. Получение списка сырья
        [HttpGet]
        public ActionResult<IEnumerable<RawMaterial>> GetRawMaterials() => Ok(RawMaterials);

        // 2. Получение конкретного сырья
        [HttpGet("{id}")]
        public ActionResult<RawMaterial> GetRawMaterial(int id)
        {
            var rm = RawMaterials.FirstOrDefault(r => r.Id == id);
            if (rm == null) return NotFound();
            return Ok(rm);
        }

        // 3. Добавление сырья
        [HttpPost]
        public ActionResult<RawMaterial> AddRawMaterial([FromBody] RawMaterial rawMaterial)
        {
            rawMaterial.Id = RawMaterials.Any() ? RawMaterials.Max(r => r.Id) + 1 : 1;
            RawMaterials.Add(rawMaterial);
            return CreatedAtAction(nameof(GetRawMaterial), new { id = rawMaterial.Id }, rawMaterial);
        }

        // 4. Изменение конкретного сырья
        [HttpPut("{id}")]
        public ActionResult<RawMaterial> UpdateRawMaterial(int id, [FromBody] RawMaterial updatedRawMaterial)
        {
            var rawMaterial = RawMaterials.FirstOrDefault(r => r.Id == id);
            if (rawMaterial == null) return NotFound();
            rawMaterial.Name = updatedRawMaterial.Name;
            rawMaterial.Place = updatedRawMaterial.Place;
            rawMaterial.Amount = updatedRawMaterial.Amount;
            rawMaterial.Price = updatedRawMaterial.Price;
            rawMaterial.Priority = updatedRawMaterial.Priority;
            return Ok(rawMaterial);
        }

        // 5. Удаление конкретного сырья
        [HttpDelete("{id}")]
        public IActionResult DeleteRawMaterial(int id)
        {
            var rawMaterial = RawMaterials.FirstOrDefault(r => r.Id == id);
            if (rawMaterial == null) return NotFound();
            RawMaterials.Remove(rawMaterial);
            return NoContent();
        }
    }

    // ============================
    // КОНТРОЛЛЕР plans (планы) - Эндпоинты 6-10
    // ============================

    [ApiController]
    [Route("api/v1/plans")]
    public class PlansController : ControllerBase
    {
        private static List<Plan> Plans = new List<Plan>
        {
            new Plan 
            { 
                Id = 1, 
                CreatedDate = DateTime.Parse("2023-09-01T12:00:00Z"),
                StartDate = DateTime.Parse("2023-09-10T08:00:00Z"),
                EndDate = DateTime.Parse("2023-09-20T18:00:00Z"),
                StrategyId = 5,
                RawMaterials = new List<PlanRawMaterial>
                {
                    new PlanRawMaterial { Id = 1, UsageAmount = 100 },
                    new PlanRawMaterial { Id = 2, UsageAmount = 200 }
                }
            }
        };

        // 6. Получение списка планов
        [HttpGet]
        public ActionResult<IEnumerable<Plan>> GetPlans() => Ok(Plans);

        // 7. Получение конкретного плана
        [HttpGet("{id}")]
        public ActionResult<Plan> GetPlan(int id)
        {
            var plan = Plans.FirstOrDefault(p => p.Id == id);
            if (plan == null) return NotFound();
            return Ok(plan);
        }

        // 8. Добавление плана
        [HttpPost]
        public ActionResult<Plan> AddPlan([FromBody] Plan plan)
        {
            plan.Id = Plans.Any() ? Plans.Max(p => p.Id) + 1 : 1;
            Plans.Add(plan);
            return CreatedAtAction(nameof(GetPlan), new { id = plan.Id }, plan);
        }

        // 9. Изменение конкретного плана
        [HttpPut("{id}")]
        public ActionResult<Plan> UpdatePlan(int id, [FromBody] Plan updatedPlan)
        {
            var plan = Plans.FirstOrDefault(p => p.Id == id);
            if (plan == null) return NotFound();
            plan.CreatedDate = updatedPlan.CreatedDate;
            plan.StartDate = updatedPlan.StartDate;
            plan.EndDate = updatedPlan.EndDate;
            plan.StrategyId = updatedPlan.StrategyId;
            plan.RawMaterials = updatedPlan.RawMaterials;
            return Ok(plan);
        }

        // 10. Удаление конкретного плана
        [HttpDelete("{id}")]
        public IActionResult DeletePlan(int id)
        {
            var plan = Plans.FirstOrDefault(p => p.Id == id);
            if (plan == null) return NotFound();
            Plans.Remove(plan);
            return NoContent();
        }
    }

    // ============================
    // КОНТРОЛЛЕР strategies (стратегии) - Эндпоинты 11-16
    // ============================

    [ApiController]
    [Route("api/v1/strategies")]
    public class StrategiesController : ControllerBase
    {
        private static List<Strategy> Strategies = new List<Strategy>
        {
            new Strategy 
            { 
                Id = 1, 
                ExecutionDate = DateTime.Parse("2023-09-30T00:00:00Z"),
                OrderIds = new List<int> { 1, 2, 3 },
                Revenue = 150000,
                Status = "active",
                Comment = "Первичная стратегия",
                ManagerId = 10
            },
            new Strategy 
            { 
                Id = 2, 
                ExecutionDate = DateTime.Parse("2023-10-15T00:00:00Z"),
                OrderIds = new List<int> { 4, 5 },
                Revenue = 90000,
                Status = "completed",
                Comment = "Выполненная стратегия",
                ManagerId = 11
            }
        };

        // 11. Получение списка стратегий
        [HttpGet]
        public ActionResult<IEnumerable<Strategy>> GetStrategies() => Ok(Strategies);

        // 12. Получение списка стратегий по id менеджера
        [HttpGet("manager/{managerId}")]
        public ActionResult<IEnumerable<Strategy>> GetStrategiesByManager(int managerId)
        {
            var strategies = Strategies.Where(s => s.ManagerId == managerId).ToList();
            return Ok(strategies);
        }

        // 13. Получение конкретной стратегии
        [HttpGet("{id}")]
        public ActionResult<Strategy> GetStrategy(int id)
        {
            var strategy = Strategies.FirstOrDefault(s => s.Id == id);
            if (strategy == null) return NotFound();
            return Ok(strategy);
        }

        // 14. Добавление стратегии
        [HttpPost]
        public ActionResult<Strategy> AddStrategy([FromBody] Strategy strategy)
        {
            strategy.Id = Strategies.Any() ? Strategies.Max(s => s.Id) + 1 : 1;
            Strategies.Add(strategy);
            return CreatedAtAction(nameof(GetStrategy), new { id = strategy.Id }, strategy);
        }

        // 15. Изменение конкретной стратегии
        [HttpPut("{id}")]
        public ActionResult<Strategy> UpdateStrategy(int id, [FromBody] Strategy updatedStrategy)
        {
            var strategy = Strategies.FirstOrDefault(s => s.Id == id);
            if (strategy == null) return NotFound();
            strategy.ExecutionDate = updatedStrategy.ExecutionDate;
            strategy.OrderIds = updatedStrategy.OrderIds;
            strategy.Revenue = updatedStrategy.Revenue;
            strategy.Status = updatedStrategy.Status;
            strategy.Comment = updatedStrategy.Comment;
            strategy.ManagerId = updatedStrategy.ManagerId;
            return Ok(strategy);
        }

        // 16. Удаление конкретной стратегии
        [HttpDelete("{id}")]
        public IActionResult DeleteStrategy(int id)
        {
            var strategy = Strategies.FirstOrDefault(s => s.Id == id);
            if (strategy == null) return NotFound();
            Strategies.Remove(strategy);
            return NoContent();
        }
    }

    // ============================
    // КОНТРОЛЛЕР clients (клиенты) - Эндпоинты 17-22
    // ============================

    [ApiController]
    [Route("api/v1/clients")]
    public class ClientsController : ControllerBase
    {
        private static List<Client> Clients = new List<Client>
        {
            new Client { Id = 1, Name = "ООО Рога и Копыта", ContactPerson = "Иван Иванов", Phone = "+7-123-456-78-90", ManagerId = 10 },
            new Client { Id = 2, Name = "ЗАО Пример", ContactPerson = "Петр Петров", Phone = "+7-987-654-32-10", ManagerId = 11 }
        };

        // 17. Получение списка клиентов
        [HttpGet]
        public ActionResult<IEnumerable<Client>> GetClients() => Ok(Clients);

        // 18. Получение конкретного клиента
        [HttpGet("{id}")]
        public ActionResult<Client> GetClient(int id)
        {
            var client = Clients.FirstOrDefault(c => c.Id == id);
            if (client == null) return NotFound();
            return Ok(client);
        }

        // 19. Добавление клиента
        [HttpPost]
        public ActionResult<Client> AddClient([FromBody] Client client)
        {
            client.Id = Clients.Any() ? Clients.Max(c => c.Id) + 1 : 1;
            Clients.Add(client);
            return CreatedAtAction(nameof(GetClient), new { id = client.Id }, client);
        }

        // 20. Изменение конкретного клиента
        [HttpPut("{id}")]
        public ActionResult<Client> UpdateClient(int id, [FromBody] Client updatedClient)
        {
            var client = Clients.FirstOrDefault(c => c.Id == id);
            if (client == null) return NotFound();
            client.Name = updatedClient.Name;
            client.ContactPerson = updatedClient.ContactPerson;
            client.Phone = updatedClient.Phone;
            client.ManagerId = updatedClient.ManagerId;
            return Ok(client);
        }

        // 21. Удаление конкретного клиента
        [HttpDelete("{id}")]
        public IActionResult DeleteClient(int id)
        {
            var client = Clients.FirstOrDefault(c => c.Id == id);
            if (client == null) return NotFound();
            Clients.Remove(client);
            return NoContent();
        }

        // 22. Получение списка клиентов по id менеджера
        [HttpGet("manager/{managerId}")]
        public ActionResult<IEnumerable<Client>> GetClientsByManager(int managerId)
        {
            var clients = Clients.Where(c => c.ManagerId == managerId).ToList();
            return Ok(clients);
        }
    }

    // ============================
    // Контроллеры для orders и sandtypes уже описаны ранее.
    // ============================

    [ApiController]
    [Route("api/v1/orders")]
    public class OrdersController : ControllerBase
    {
        private static List<Order> Orders = new List<Order>
        {
            new Order 
            { 
                Id = 1, ClientId = 1, 
                OrderDate = DateTime.Parse("2023-09-01T12:00:00Z"), 
                ExecutionDate = DateTime.Parse("2023-09-05T12:00:00Z"), 
                SupplyVolume = 500, UpperFraction = 80, LowerFraction = 40, 
                OrderAmount = 100000, Status = "pending", Comment = "Urgent order" 
            },
            new Order 
            { 
                Id = 2, ClientId = 2, 
                OrderDate = DateTime.Parse("2023-09-02T12:00:00Z"), 
                ExecutionDate = DateTime.Parse("2023-09-06T12:00:00Z"), 
                SupplyVolume = 300, UpperFraction = 75, LowerFraction = 35, 
                OrderAmount = 60000, Status = "completed", Comment = "Standard order" 
            }
        };

        // 23. Получение списка всех заказов
        [HttpGet]
        public ActionResult<IEnumerable<Order>> GetOrders()
        {
            return Ok(Orders);
        }

        // 24. Получение списка заказов по id клиента
        [HttpGet("client/{clientId}")]
        public ActionResult<IEnumerable<Order>> GetOrdersByClient(int clientId)
        {
            var clientOrders = Orders.Where(o => o.ClientId == clientId).ToList();
            return Ok(clientOrders);
        }

        // 25. Получение конкретного заказа
        [HttpGet("{id}")]
        public ActionResult<Order> GetOrder(int id)
        {
            var order = Orders.FirstOrDefault(o => o.Id == id);
            if (order == null)
                return NotFound();
            return Ok(order);
        }

        // 26. Добавление заказа
        [HttpPost]
        public ActionResult<Order> CreateOrder([FromBody] Order order)
        {
            order.Id = Orders.Any() ? Orders.Max(o => o.Id) + 1 : 1;
            Orders.Add(order);
            return CreatedAtAction(nameof(GetOrder), new { id = order.Id }, order);
        }

        // 27. Изменение конкретного заказа
        [HttpPut("{id}")]
        public ActionResult<Order> UpdateOrder(int id, [FromBody] Order updatedOrder)
        {
            var order = Orders.FirstOrDefault(o => o.Id == id);
            if (order == null)
                return NotFound();
            
            order.ClientId = updatedOrder.ClientId;
            order.OrderDate = updatedOrder.OrderDate;
            order.ExecutionDate = updatedOrder.ExecutionDate;
            order.SupplyVolume = updatedOrder.SupplyVolume;
            order.UpperFraction = updatedOrder.UpperFraction;
            order.LowerFraction = updatedOrder.LowerFraction;
            order.OrderAmount = updatedOrder.OrderAmount;
            order.Status = updatedOrder.Status;
            order.Comment = updatedOrder.Comment;
            
            return Ok(order);
        }

        // 28. Удаление конкретного заказа
        [HttpDelete("{id}")]
        public IActionResult DeleteOrder(int id)
        {
            var order = Orders.FirstOrDefault(o => o.Id == id);
            if (order == null)
                return NotFound();
            
            Orders.Remove(order);
            return NoContent();
        }
    }

    // Контроллер для работы с типами песков, ассоциированными с сырьём
    [ApiController]
    [Route("api/v1/sandtypes/rawmaterial/{rawMaterialId}")]
    public class SandTypesController : ControllerBase
    {
        // in-memory хранилище для типов песков
        private static List<SandType> SandTypes = new List<SandType>
        {
            new SandType { Id = 1, RawMaterialId = 10, UpperFraction = 90, SandContent = 350 },
            new SandType { Id = 2, RawMaterialId = 10, UpperFraction = 85, SandContent = 320 }
        };

        // 29. Получение списка типов песков по id сырья
        [HttpGet]
        public ActionResult<IEnumerable<SandType>> GetSandTypes(int rawMaterialId)
        {
            var types = SandTypes.Where(s => s.RawMaterialId == rawMaterialId).ToList();
            return Ok(types);
        }

        // 30. Добавление типа песков по id сырья
        [HttpPost]
        public ActionResult<SandType> CreateSandType(int rawMaterialId, [FromBody] SandType sandType)
        {
            sandType.RawMaterialId = rawMaterialId;
            sandType.Id = SandTypes.Any() ? SandTypes.Max(s => s.Id) + 1 : 1;
            SandTypes.Add(sandType);
            return CreatedAtAction(nameof(GetSandTypes), new { rawMaterialId = rawMaterialId }, sandType);
        }

        // 31. Изменение типа песков по id сырья
        [HttpPut]
        public ActionResult<SandType> UpdateSandType(int rawMaterialId, [FromBody] SandType updatedSandType)
        {
            var sandType = SandTypes.FirstOrDefault(s => s.RawMaterialId == rawMaterialId && s.Id == updatedSandType.Id);
            if (sandType == null)
                return NotFound();
            
            sandType.UpperFraction = updatedSandType.UpperFraction;
            sandType.SandContent = updatedSandType.SandContent;
            
            return Ok(sandType);
        }

        // 32. Удаление типа песков по id сырья
        // Здесь используем Query-параметр для typeId
        [HttpDelete]
        public IActionResult DeleteSandType(int rawMaterialId, [FromQuery] int typeId)
        {
            var sandType = SandTypes.FirstOrDefault(s => s.RawMaterialId == rawMaterialId && s.Id == typeId);
            if (sandType == null)
                return NotFound();
            
            SandTypes.Remove(sandType);
            return NoContent();
        }
    }
}

```



## Тестирование API  

Тестирование проводилось с использованием **Postman**.
![screenshot from Postman]()
