# Использование принципов проектирования на уровне методов и классов

## Цель работы
Получить опыт проектирования и реализации модулей с использованием принципов KISS, YAGNI, DRY, SOLID и др.

---

## 1. Диаграмма контейнеров
![](https://github.com/ChirkovaDS/DeepSand/blob/dev/LAB2/%D0%94%D0%B8%D0%B0%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B0%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B8%CC%86%D0%BD%D0%B5%D1%80%D0%BE%D0%B2%20(1).jpg)

**Описание контейнеров:**
см LAB2/README.md

## 2. Диаграмма компонентов
![](https://github.com/ChirkovaDS/DeepSand/blob/dev/LAB2/%D0%94%D0%B8%D0%B0%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B0%20%D0%BA%D0%BE%D0%BC%D0%BF%D0%BE%D0%BD%D0%B5%D0%BD%D1%82%D0%BE%D0%B2%20(2).jpg)

**Описание компонентов:**
см LAB3/README.md


## 3. Диаграмма последовательностей основных прецедентов
### 3.1. Добавления сырья с новой номенклатурой
![Добавления сырья с новой номенклатурой (Лаборант)](https://github.com/ChirkovaDS/DeepSand/blob/dev/LAB3/SequenceLaborant.png)

### 3.2. Изменение стратегии продаж после добавления нового клиента (Менеджер)
![Изменение стратегии продаж (Менеджер)](https://github.com/ChirkovaDS/DeepSand/blob/dev/LAB3/SequenceManager.png)

### 3.3. Планирование производства по стратегии продаж (Технолог)
![Планирование производства по стратегии продаж (Технолог)](https://github.com/ChirkovaDS/DeepSand/blob/dev/LAB3/SequenceTech.png)

### 3.4. Контроль складких остатков (Логист)
![Контроль складких остатков (Логист)](https://github.com/ChirkovaDS/DeepSand/blob/dev/LAB3/SequenceLog.png)

## 4. Модель БД
![](https://github.com/ChirkovaDS/DeepSand/blob/dev/LAB3/%D0%91%D0%94.jpg)

## 5. Применение основных принципов разработки

### 5.1. KISS для модуля управления клиентской базой

```csharp
// Модель клиента
public class Client
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}

// Простой сервис управления клиентами, реализующий только базовые CRUD-операции
public class ClientManagementService
{
    // Используем простой список для хранения клиентов (в реальной системе – обращение к БД)
    private readonly List<Client> _clients = new List<Client>();

    public void AddClient(Client client)
    {
        _clients.Add(client);
    }

    public void UpdateClient(Client client)
    {
        var existing = _clients.FirstOrDefault(c => c.Id == client.Id);
        if (existing != null)
        {
            existing.Name = client.Name;
            existing.Email = client.Email;
        }
    }

    public void DeleteClient(int clientId)
    {
        var client = _clients.FirstOrDefault(c => c.Id == clientId);
        if (client != null)
        {
            _clients.Remove(client);
        }
    }

    // Для демонстрации получения всех клиентов
    public List<Client> GetAllClients() => _clients;
}
```
В данном модуле код максимально прост (Keep It Simple, Stupid): операции добавления, обновления и удаления реализованы без излишних абстракций и функционала, которого не требует бизнес-задача.

### 5.2. Принцип YAGNI для модуля формирования отчётов

```csharp
using System.IO;

public class ReportGenerationService
{
    // Генерация отчёта в формате CSV из списка клиентов
    public void GenerateCsvReport(List<Client> clients, string filePath)
    {
        using (var writer = new StreamWriter(filePath))
        {
            // Заголовок CSV
            writer.WriteLine("Id,Name,Email");
            // Запись данных клиентов
            foreach (var client in clients)
            {
                writer.WriteLine($"{client.Id},{client.Name},{client.Email}");
            }
        }
    }
}
```

Принцип YAGNI (You Ain't Gonna Need It) означает, что реализуем только необходимый функционал – в данном случае поддерживается только CSV, без реализации других форматов (например, PDF или Excel), что упрощает разработку.

### 5.3. Принцип DRY для взаимодействия модуля сегментации клиентов и модуля управления клиентской базой с БД

```csharp
// Интерфейс репозитория для клиентов
public interface IClientRepository
{
    List<Client> GetAllClients();
}

// Общая реализация репозитория (симуляция обращения к БД)
public class ClientRepository : IClientRepository
{
    // Симулируем данные БД
    private readonly List<Client> _clients = new List<Client>
    {
        new Client { Id = 1, Name = "Client One", Email = "client1@example.com" },
        new Client { Id = 2, Name = "Client Two", Email = "client2@example.com" }
    };

    public List<Client> GetAllClients() => _clients;
}

// Модуль сегментации клиентов использует единый репозиторий для получения данных
public class ClientSegmentationService
{
    private readonly IClientRepository _repository;
    public ClientSegmentationService(IClientRepository repository)
    {
        _repository = repository;
    }

    // Метод сегментации по заданному критерию
    public List<Client> SegmentClients(Func<Client, bool> criteria)
    {
        return _repository.GetAllClients().Where(criteria).ToList();
    }
}

// Модуль управления клиентской базой может также использовать тот же репозиторий при необходимости
public class ClientManagementServiceDRY
{
    private readonly IClientRepository _repository;
    public ClientManagementServiceDRY(IClientRepository repository)
    {
        _repository = repository;
    }

    public void AddClient(Client client)
    {
        var clients = _repository.GetAllClients();
        clients.Add(client);
    }

    // Другие операции управления могут использовать _repository.GetAllClients()
}
```
Здесь принцип DRY (Don't Repeat Yourself) реализован через общий интерфейс и реализацию репозитория. Модули сегментации и управления клиентской базой получают данные из одного источника, что исключает дублирование кода при выполнении запросов к БД.


### 5.4. Принцип SOLID для модуля анализа состава сырья

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

namespace ClientSegmentationModule
{
    // Модель клиента
    public class Client
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public decimal HypotheticalProfit { get; set; }
        public List<string> RequiredSandNomenclatures { get; set; }
        public int GeneralPriority { get; set; }
    }

    // ISP: Интерфейс, определяющий контракт для оценки клиента по одному критерию.
    public interface IClientSegmentationCriteria
    {
        // Метод возвращает строковое значение группы, к которой относится клиент
        string Evaluate(Client client);
    }

    // SRP: Класс для сегментации по гипотетической прибыли
    public class HypotheticalProfitCriteria : IClientSegmentationCriteria
    {
        public string Evaluate(Client client)
        {
            // Если гипотетическая прибыль больше 10,000, отнесем клиента к группе HighProfit, иначе к LowProfit
            return client.HypotheticalProfit > 10000 ? "HighProfit" : "LowProfit";
        }
    }

    // SRP: Класс для сегментации по требуемым номенклатурам песков
    public class RequiredSandNomenclaturesCriteria : IClientSegmentationCriteria
    {
        public string Evaluate(Client client)
        {
            // Если клиент требует более 3 номенклатур, отнесем к группе DiverseRequirement, иначе к LimitedRequirement
            return (client.RequiredSandNomenclatures != null && client.RequiredSandNomenclatures.Count > 3)
                ? "DiverseRequirement"
                : "LimitedRequirement";
        }
    }

    // SRP: Класс для сегментации по общему приоритету
    public class GeneralPriorityCriteria : IClientSegmentationCriteria
    {
        public string Evaluate(Client client)
        {
            // Если приоритет 5 или выше, клиент относится к группе HighPriority, иначе LowPriority
            return client.GeneralPriority >= 5 ? "HighPriority" : "LowPriority";
        }
    }

    // Клиентский сервис сегментации использует DIP: зависит от абстракции IClientSegmentationCriteria
    public class ClientSegmentationService
    {
        // Метод сегментации, который принимает список клиентов и критерий сегментации
        public Dictionary<string, List<Client>> SegmentClients(List<Client> clients, IClientSegmentationCriteria criteria)
        {
            var groups = new Dictionary<string, List<Client>>();

            foreach (var client in clients)
            {
                var group = criteria.Evaluate(client);

                if (!groups.ContainsKey(group))
                {
                    groups[group] = new List<Client>();
                }

                groups[group].Add(client);
            }

            return groups;
        }
    }

    // Пример использования
    public class Program
    {
        public static void Main(string[] args)
        {
            // Подготовка тестовых данных
            var clients = new List<Client>
            {
                new Client { Id = 1, Name = "Alice", HypotheticalProfit = 15000, RequiredSandNomenclatures = new List<string> { "TypeA", "TypeB" }, GeneralPriority = 6 },
                new Client { Id = 2, Name = "Bob", HypotheticalProfit = 8000, RequiredSandNomenclatures = new List<string> { "TypeA" }, GeneralPriority = 4 },
                new Client { Id = 3, Name = "Charlie", HypotheticalProfit = 20000, RequiredSandNomenclatures = new List<string> { "TypeA", "TypeB", "TypeC", "TypeD" }, GeneralPriority = 7 }
            };

            // Создаем сервис сегментации
            var segmentationService = new ClientSegmentationService();

            // Пример сегментации по гипотетической прибыли
            IClientSegmentationCriteria profitCriteria = new HypotheticalProfitCriteria();
            var profitGroups = segmentationService.SegmentClients(clients, profitCriteria);
            Console.WriteLine("Сегментация по гипотетической прибыли:");
            foreach (var group in profitGroups)
            {
                Console.WriteLine($"Группа: {group.Key}");
                foreach (var client in group.Value)
                {
                    Console.WriteLine($"  Клиент: {client.Name}");
                }
            }

            // Пример сегментации по требуемым номенклатурам песков
            IClientSegmentationCriteria sandCriteria = new RequiredSandNomenclaturesCriteria();
            var sandGroups = segmentationService.SegmentClients(clients, sandCriteria);
            Console.WriteLine("\nСегментация по требуемым номенклатурам песков:");
            foreach (var group in sandGroups)
            {
                Console.WriteLine($"Группа: {group.Key}");
                foreach (var client in group.Value)
                {
                    Console.WriteLine($"  Клиент: {client.Name}");
                }
            }

            // Пример сегментации по общему приоритету
            IClientSegmentationCriteria priorityCriteria = new GeneralPriorityCriteria();
            var priorityGroups = segmentationService.SegmentClients(clients, priorityCriteria);
            Console.WriteLine("\nСегментация по общему приоритету:");
            foreach (var group in priorityGroups)
            {
                Console.WriteLine($"Группа: {group.Key}");
                foreach (var client in group.Value)
                {
                    Console.WriteLine($"  Клиент: {client.Name}");
                }
            }
        }
    }
}
```

### Объяснение:

1. **S (Single Responsibility):**  
   Каждый класс, реализующий интерфейс `IClientSegmentationCriteria` (например, `HypotheticalProfitCriteria`, `RequiredSandNomenclaturesCriteria`, `GeneralPriorityCriteria`), отвечает только за вычисление одного критерия для сегментации клиента.

2. **O (Open/Closed):**  
   Модуль сегментации клиентов (в данном случае класс `ClientSegmentationService`) использует интерфейс `IClientSegmentationCriteria`. Для добавления нового критерия достаточно создать новый класс, реализующий этот интерфейс, не изменяя существующий код сервиса.

3. **L (Liskov Substitution):**  
   Все реализации интерфейса `IClientSegmentationCriteria` могут быть взаимозаменяемыми. Клиентский сервис сегментации не зависит от конкретной реализации, а работает с любой реализацией данного интерфейса.

4. **I (Interface Segregation):**  
   Интерфейс `IClientSegmentationCriteria` содержит только один метод `Evaluate(Client client)`, что гарантирует, что потребители данного интерфейса не вынуждены зависеть от методов, которые им не нужны.

5. **D (Dependency Inversion):**  
   Класс `ClientSegmentationService` зависит от абстракции (`IClientSegmentationCriteria`), а не от конкретных реализаций. Это позволяет легко подменять реализации для тестирования или расширения функционала.


### 5.5. BDUF (Big Design Up Front)  
**Рекомендуется к применению** 
**Обоснование:**  
- Задает четкую архитектуру с выделенными модулями (склад, производство, клиентская база, аналитика), которых много 
- Позволяет увидеть взаимосвязи между подсистемами.  
- Ограничен изменчивостью требований и риском излишней сложности, однако поддержка системы с точки зрения развития не требуется  

### 5.6. SoC (Separation of Concerns)  
**Рекомендуется к применению**
**Обоснование:**  
- Разделение на четкие слои: веб-интерфейс, бизнес-логика, база данных, аналитический сервис.  
- Обеспечивает независимое развитие, тестирование и масштабирование каждого модуля.


### 5.7. PoC (Proof of Concept)  
**Рекомендуется к применению**, но не будет реализован см пункт 5.8
**Обоснование:**  
- Проверка ключевых функций (планирование производства, аналитика, сегментация клиентов).  
- Валидация интеграционных сценариев и производительности критичных модулей.  


### 5.8. MVP (Minimum Viable Product)  
**Рекомендуется к применению** и уже реализовано
**Обоснование:**  
- Позволяет быстро получить обратную связь по ключевым функциям (управление складом, клиентской базой, аналитикой).  
- Обеспечивает постепенное расширение функционала по мере уточнения требований.  
- Сфокусирован на реализации минимально жизнеспособного продукта с последующим развитием.
**Решение:** Создание прототипа с минимальным функционалом и его пилотное тестирование с последующей итеративной доработкой (что было сделано в рамках ВКР на 3ем курсе) 

