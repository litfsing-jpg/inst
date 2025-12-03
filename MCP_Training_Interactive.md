# 🎓 ИНТЕРАКТИВНЫЙ ТРЕНИНГ: MODEL CONTEXT PROTOCOL (MCP)
## Обучающая программа с практическими заданиями

---

## 📋 СТРУКТУРА ТРЕНИНГА

### МОДУЛЬ 1: Основы MCP (30 минут)
### МОДУЛЬ 2: Архитектура и компоненты (45 минут)
### МОДУЛЬ 3: Практическая разработка (60 минут)
### МОДУЛЬ 4: Production и безопасность (45 минут)

---

# МОДУЛЬ 1: ОСНОВЫ MCP

## 🎯 Цели модуля:
- Понять, что такое MCP и зачем он нужен
- Разобраться в основных концепциях
- Научиться объяснять MCP клиентам/коллегам

---

## 📖 ТЕОРИЯ (10 минут)

### Что такое MCP?

**Model Context Protocol (MCP)** - это открытый стандарт для подключения AI приложений к внешним системам.

**Аналогия:** USB-C для AI приложений - универсальный способ подключения данных к AI.

### Главная проблема, которую решает MCP

**ДО MCP:**
```
AI приложение → Кастомный коннектор 1 → База данных
               → Кастомный коннектор 2 → API
               → Кастомный коннектор 3 → Файлы
               → Кастомный коннектор 4 → CRM
```
❌ Каждая интеграция = новый код
❌ Нет стандартизации
❌ Сложная поддержка

**С MCP:**
```
AI приложение → MCP Client → MCP Server 1 → База данных
                          → MCP Server 2 → API
                          → MCP Server 3 → Файлы
                          → MCP Server 4 → CRM
```
✅ Единый стандарт
✅ Переиспользуемые серверы
✅ Простая поддержка

---

## 🧠 КВИЗ 1: Проверка базовых знаний

### Вопрос 1
**Что такое MCP простыми словами?**

A) Новая AI модель от Anthropic
B) Протокол для подключения AI к данным
C) Язык программирования
D) База данных

<details>
<summary>✅ Ответ</summary>

**B) Протокол для подключения AI к данным**

Объяснение: MCP - это стандарт (протокол), который позволяет AI приложениям подключаться к различным источникам данных через единый интерфейс.
</details>

### Вопрос 2
**Какую главную проблему решает MCP?**

A) Делает AI модели умнее
B) Ускоряет работу компьютера
C) Упрощает интеграцию AI с данными
D) Снижает стоимость облачных сервисов

<details>
<summary>✅ Ответ</summary>

**C) Упрощает интеграцию AI с данными**

Объяснение: MCP стандартизирует способ подключения AI к разным источникам данных, избавляя от необходимости писать кастомные коннекторы для каждого источника.
</details>

### Вопрос 3
**С чем можно сравнить MCP?**

A) USB-C порт - универсальный способ подключения
B) Процессор компьютера
C) Жесткий диск
D) Клавиатура

<details>
<summary>✅ Ответ</summary>

**A) USB-C порт - универсальный способ подключения**

Объяснение: Как USB-C - это стандартный разъем для разных устройств, так и MCP - это стандартный протокол для подключения разных источников данных к AI.
</details>

---

## 💼 ПРАКТИЧЕСКОЕ ЗАДАНИЕ 1: Elevator Pitch

**Время: 5 минут**

**Задача:** Представьте, что вы едете в лифте с потенциальным клиентом. У вас есть 30 секунд, чтобы объяснить, что такое MCP и почему это важно для его бизнеса.

**Напишите свой elevator pitch:**

```
Мой Elevator Pitch:
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**Пример хорошего питча:**

> "Представьте, что ваш AI-ассистент может автоматически получать данные из всех ваших систем - CRM, базы данных, файлов, API - через единый стандартный интерфейс. Это MCP. Вместо месяцев на разработку кастомных интеграций, вы можете подключить готовые решения за часы. Block (компания Square) внедрила MCP и их сотрудники теперь экономят 50-75% времени на рутинных задачах."

---

## 🎯 ПРАКТИЧЕСКОЕ ЗАДАНИЕ 2: Поиск применения

**Время: 10 минут**

**Задача:** Придумайте 3 конкретных сценария применения MCP для ВАШЕГО бизнеса или клиента.

### Шаблон для заполнения:

**Сценарий 1:**
- **Индустрия:** _______________________
- **Проблема:** _______________________
- **Решение с MCP:** _______________________
- **Источники данных:** _______________________
- **Ожидаемый результат:** _______________________

**Сценарий 2:**
- **Индустрия:** _______________________
- **Проблема:** _______________________
- **Решение с MCP:** _______________________
- **Источники данных:** _______________________
- **Ожидаемый результат:** _______________________

**Сценарий 3:**
- **Индустрия:** _______________________
- **Проблема:** _______________________
- **Решение с MCP:** _______________________
- **Источники данных:** _______________________
- **Ожидаемый результат:** _______________________

---

# МОДУЛЬ 2: АРХИТЕКТУРА И КОМПОНЕНТЫ

## 🎯 Цели модуля:
- Понять архитектуру MCP
- Разобраться в ролях компонентов
- Научиться проектировать MCP решения

---

## 📖 ТЕОРИЯ (15 минут)

### Трехуровневая архитектура MCP

```
┌─────────────────────────────────────────────┐
│     MCP HOST (Claude Desktop, VS Code)      │  ← Координатор
│     Управляет всеми соединениями            │
└──────────────┬──────────────┬───────────────┘
               │              │
        ┌──────▼──────┐  ┌───▼──────────┐
        │  MCP Client │  │  MCP Client  │        ← Посредники
        │      1      │  │      2       │
        └──────┬──────┘  └───┬──────────┘
               │             │
        ┌──────▼──────┐  ┌───▼──────────┐
        │ MCP Server 1│  │ MCP Server 2 │        ← Поставщики данных
        │  (Database) │  │  (Files)     │
        └─────────────┘  └──────────────┘
```

### 3 главных компонента:

#### 1. MCP HOST (Хост)
**Роль:** Координатор всего процесса
- Приложение, которое использует пользователь (Claude Desktop, VS Code)
- Управляет несколькими клиентами
- Предоставляет UI

**Примеры:**
- Claude Desktop
- VS Code с расширением
- Кастомное AI приложение

#### 2. MCP CLIENT (Клиент)
**Роль:** Посредник между хостом и сервером
- Один клиент = одно соединение с сервером
- Обеспечивает изоляцию соединений
- Управляет безопасностью

**Важно:** Каждому серверу - свой клиент!

#### 3. MCP SERVER (Сервер)
**Роль:** Поставщик данных и инструментов
- Предоставляет доступ к данным
- Открывает инструменты (tools)
- Предоставляет ресурсы (resources)

**Примеры:**
- Сервер для работы с базой данных
- Сервер для работы с файлами
- Сервер для интеграции с API

### 3 примитива MCP Server:

#### 1. **TOOLS (Инструменты)**
- Выполняемые функции
- AI модель решает, когда их вызывать
- Примеры: `create_file()`, `send_email()`, `query_database()`

#### 2. **RESOURCES (Ресурсы)**
- Read-only источники данных
- Для контекста AI модели
- Примеры: файлы, схемы БД, документация

#### 3. **PROMPTS (Подсказки)**
- Переиспользуемые шаблоны инструкций
- Пользователь явно их выбирает
- Примеры: "Проанализируй продажи", "Создай отчет"

---

## 🧠 КВИЗ 2: Архитектура

### Вопрос 1
**Какой компонент является координатором всей системы?**

A) MCP Server
B) MCP Client
C) MCP Host
D) MCP Tool

<details>
<summary>✅ Ответ</summary>

**C) MCP Host**

Объяснение: Host - это приложение верхнего уровня (например, Claude Desktop), которое координирует все соединения и управляет клиентами.
</details>

### Вопрос 2
**Сколько клиентов нужно для подключения к 5 разным серверам?**

A) 1 клиент на все серверы
B) 5 клиентов - по одному на каждый сервер
C) 2 клиента - один для чтения, один для записи
D) Зависит от типа данных

<details>
<summary>✅ Ответ</summary>

**B) 5 клиентов - по одному на каждый сервер**

Объяснение: Каждый клиент поддерживает выделенное соединение один-к-одному с сервером. Это обеспечивает изоляцию и безопасность.
</details>

### Вопрос 3
**Что такое "Tool" в контексте MCP?**

A) Программа для разработки
B) Выполняемая функция, которую может вызвать AI
C) Источник данных только для чтения
D) Шаблон промпта

<details>
<summary>✅ Ответ</summary>

**B) Выполняемая функция, которую может вызвать AI**

Объяснение: Tools - это функции, которые AI модель может вызывать для выполнения действий (создать файл, отправить email, сделать запрос к БД и т.д.).
</details>

### Вопрос 4
**В чем разница между Tool и Resource?**

A) Tool = для чтения, Resource = для записи
B) Tool = выполняет действия, Resource = предоставляет данные
C) Tool = для баз данных, Resource = для файлов
D) Нет разницы, это синонимы

<details>
<summary>✅ Ответ</summary>

**B) Tool = выполняет действия, Resource = предоставляет данные**

Объяснение: Tools - это активные функции для выполнения операций, а Resources - это пассивные источники данных только для чтения.
</details>

---

## 💼 ПРАКТИЧЕСКОЕ ЗАДАНИЕ 3: Проектирование архитектуры

**Время: 15 минут**

**Задача:** Спроектируйте MCP архитектуру для следующего сценария:

### Сценарий:
Маркетинговое агентство хочет создать AI-ассистента для автоматизации работы с рекламными кампаниями. Ассистент должен:
1. Получать данные о кампаниях из Яндекс.Директ
2. Анализировать статистику из Google Analytics
3. Создавать отчеты в Google Sheets
4. Отправлять уведомления в Slack
5. Хранить историю в PostgreSQL

### Ваше задание:
Нарисуйте (текстом) архитектуру решения, указав:
- MCP Host (какое приложение)
- Все необходимые MCP Servers
- Для каждого сервера укажите Tools и Resources

```
МОЕ РЕШЕНИЕ:

MCP HOST:
_____________________________________________________________

MCP SERVERS:

Сервер 1: _____________________
  Tools:
    -
    -
  Resources:
    -
    -

Сервер 2: _____________________
  Tools:
    -
    -
  Resources:
    -
    -

[Продолжите для всех серверов...]
```

**Пример правильного решения:**

<details>
<summary>Показать решение</summary>

```
MCP HOST:
- Claude Desktop (координирует все серверы)

MCP SERVERS:

Сервер 1: yandex-direct-mcp
  Tools:
    - get_campaigns() - получить список кампаний
    - get_campaign_stats() - получить статистику кампании
    - pause_campaign() - приостановить кампанию
    - update_bid() - обновить ставку
  Resources:
    - campaign_list - список всех кампаний
    - account_info - информация об аккаунте

Сервер 2: google-analytics-mcp
  Tools:
    - run_report() - запустить отчет
  Resources:
    - traffic_data - данные о трафике
    - conversion_data - данные о конверсиях

Сервер 3: google-sheets-mcp
  Tools:
    - create_sheet() - создать таблицу
    - write_data() - записать данные
    - format_cells() - форматировать ячейки
  Resources:
    - sheet_templates - шаблоны таблиц

Сервер 4: slack-mcp
  Tools:
    - send_message() - отправить сообщение
    - create_channel() - создать канал
  Resources:
    - channel_list - список каналов

Сервер 5: postgresql-mcp
  Tools:
    - execute_query() - выполнить запрос
    - insert_data() - вставить данные
  Resources:
    - database_schema - схема базы данных
    - query_history - история запросов
```
</details>

---

# МОДУЛЬ 3: ПРАКТИЧЕСКАЯ РАЗРАБОТКА

## 🎯 Цели модуля:
- Научиться создавать MCP Server
- Понять работу с JSON-RPC протоколом
- Написать простой рабочий пример

---

## 📖 ТЕОРИЯ (15 минут)

### Как создать MCP Server

#### Шаг 1: Инициализация

```python
from mcp import Server
from mcp.types import TextContent, Tool

# Создаем сервер с уникальным именем
server = Server("my-awesome-service")
```

#### Шаг 2: Определение инструментов (Tools)

```python
@server.tool()
def calculate_roi(revenue: float, cost: float) -> dict:
    """
    Рассчитать ROI для рекламной кампании.

    Args:
        revenue: Выручка от кампании
        cost: Затраты на кампанию

    Returns:
        ROI в процентах и абсолютная прибыль
    """
    roi = ((revenue - cost) / cost) * 100
    profit = revenue - cost

    return {
        "roi_percent": round(roi, 2),
        "profit": round(profit, 2)
    }
```

#### Шаг 3: Определение ресурсов (Resources)

```python
@server.resource("analytics://monthly-report")
def get_monthly_report() -> str:
    """
    Получить ежемесячный отчет по аналитике.
    """
    # Здесь код получения данных
    return report_data
```

#### Шаг 4: Определение подсказок (Prompts)

```python
@server.prompt("analyze-campaign")
def campaign_analysis_prompt():
    """
    Шаблон для анализа рекламной кампании.
    """
    return {
        "messages": [
            {
                "role": "user",
                "content": "Проанализируй эффективность кампании..."
            }
        ]
    }
```

#### Шаг 5: Запуск сервера

```python
if __name__ == "__main__":
    server.run()
```

### ⚠️ КРИТИЧЕСКИЕ ПРАВИЛА

#### ❌ НИКОГДА НЕ ДЕЛАЙТЕ:

```python
# НЕПРАВИЛЬНО - сломает JSON-RPC протокол!
@server.tool()
def bad_example():
    print("This will break everything!")  # ❌ НЕТ print()!
    return result
```

#### ✅ ПРАВИЛЬНО:

```python
import logging

logger = logging.getLogger(__name__)

@server.tool()
def good_example():
    logger.info("This is correct")  # ✅ logging в stderr
    return result
```

### Почему нельзя использовать print()?

MCP использует **stdout (стандартный вывод)** для передачи JSON-RPC сообщений. Любой `print()` запишет текст в stdout и **сломает JSON протокол**.

---

## 🧠 КВИЗ 3: Разработка

### Вопрос 1
**Что произойдет, если использовать print() в MCP Server?**

A) Ничего страшного
B) Сервер упадет с ошибкой
C) JSON-RPC протокол сломается
D) Сообщение выведется в лог

<details>
<summary>✅ Ответ</summary>

**C) JSON-RPC протокол сломается**

Объяснение: MCP использует stdout для передачи JSON-RPC сообщений. print() запишет текст в stdout и испортит JSON, что приведет к ошибкам протокола.
</details>

### Вопрос 2
**Что правильно использовать для отладки в MCP Server?**

A) print()
B) console.log()
C) logging в stderr
D) echo

<details>
<summary>✅ Ответ</summary>

**C) logging в stderr**

Объяснение: Для отладки нужно использовать logging, который пишет в stderr, не затрагивая stdout с JSON-RPC сообщениями.
</details>

### Вопрос 3
**Какой декоратор используется для определения инструмента?**

A) @server.function()
B) @server.tool()
C) @server.method()
D) @server.api()

<details>
<summary>✅ Ответ</summary>

**B) @server.tool()**

Объяснение: Декоратор @server.tool() используется для регистрации функции как инструмента MCP.
</details>

---

## 💼 ПРАКТИЧЕСКОЕ ЗАДАНИЕ 4: Создание MCP Server

**Время: 30 минут**

**Задача:** Создайте простой MCP Server для работы с маркетинговыми метриками.

### Требования:

**Tools (3 инструмента):**
1. `calculate_ctr(impressions, clicks)` - рассчитать CTR
2. `calculate_cpc(cost, clicks)` - рассчитать CPC
3. `calculate_conversion_rate(clicks, conversions)` - рассчитать конверсию

**Resources (1 ресурс):**
1. `metrics://formulas` - справка с формулами метрик

**Prompts (1 промпт):**
1. `analyze-metrics` - шаблон для анализа метрик

### Шаблон для работы:

```python
from mcp import Server
from mcp.types import TextContent, Tool
import logging

logger = logging.getLogger(__name__)

# 1. Создайте сервер
server = Server("marketing-metrics")

# 2. Реализуйте Tool 1: calculate_ctr
@server.tool()
def calculate_ctr(impressions: int, clicks: int) -> dict:
    """
    Рассчитать CTR (Click-Through Rate).

    Args:
        impressions: Количество показов
        clicks: Количество кликов

    Returns:
        CTR в процентах
    """
    # ВАШ КОД ЗДЕСЬ
    pass

# 3. Реализуйте Tool 2: calculate_cpc
# ВАШ КОД ЗДЕСЬ

# 4. Реализуйте Tool 3: calculate_conversion_rate
# ВАШ КОД ЗДЕСЬ

# 5. Реализуйте Resource: metrics formulas
# ВАШ КОД ЗДЕСЬ

# 6. Реализуйте Prompt: analyze-metrics
# ВАШ КОД ЗДЕСЬ

# 7. Запуск сервера
if __name__ == "__main__":
    server.run()
```

**Пример решения:**

<details>
<summary>Показать решение</summary>

```python
from mcp import Server
from mcp.types import TextContent, Tool
import logging

logger = logging.getLogger(__name__)

server = Server("marketing-metrics")

@server.tool()
def calculate_ctr(impressions: int, clicks: int) -> dict:
    """
    Рассчитать CTR (Click-Through Rate).

    Args:
        impressions: Количество показов
        clicks: Количество кликов

    Returns:
        CTR в процентах
    """
    if impressions == 0:
        return {"error": "Количество показов не может быть 0"}

    ctr = (clicks / impressions) * 100

    logger.info(f"Calculated CTR: {ctr}%")

    return {
        "ctr_percent": round(ctr, 2),
        "impressions": impressions,
        "clicks": clicks
    }

@server.tool()
def calculate_cpc(cost: float, clicks: int) -> dict:
    """
    Рассчитать CPC (Cost Per Click).

    Args:
        cost: Общие затраты
        clicks: Количество кликов

    Returns:
        CPC
    """
    if clicks == 0:
        return {"error": "Количество кликов не может быть 0"}

    cpc = cost / clicks

    logger.info(f"Calculated CPC: {cpc}")

    return {
        "cpc": round(cpc, 2),
        "total_cost": cost,
        "total_clicks": clicks
    }

@server.tool()
def calculate_conversion_rate(clicks: int, conversions: int) -> dict:
    """
    Рассчитать коэффициент конверсии.

    Args:
        clicks: Количество кликов
        conversions: Количество конверсий

    Returns:
        Conversion rate в процентах
    """
    if clicks == 0:
        return {"error": "Количество кликов не может быть 0"}

    conv_rate = (conversions / clicks) * 100

    logger.info(f"Calculated Conversion Rate: {conv_rate}%")

    return {
        "conversion_rate_percent": round(conv_rate, 2),
        "total_clicks": clicks,
        "total_conversions": conversions
    }

@server.resource("metrics://formulas")
def get_metrics_formulas() -> str:
    """
    Справка с формулами маркетинговых метрик.
    """
    return """
    МАРКЕТИНГОВЫЕ МЕТРИКИ - ФОРМУЛЫ

    1. CTR (Click-Through Rate)
       Формула: (Clicks / Impressions) × 100%
       Пример: (150 / 10000) × 100% = 1.5%

    2. CPC (Cost Per Click)
       Формула: Total Cost / Total Clicks
       Пример: 5000 руб / 150 кликов = 33.33 руб

    3. Conversion Rate
       Формула: (Conversions / Clicks) × 100%
       Пример: (15 / 150) × 100% = 10%

    4. ROI (Return on Investment)
       Формула: ((Revenue - Cost) / Cost) × 100%
       Пример: ((25000 - 5000) / 5000) × 100% = 400%
    """

@server.prompt("analyze-metrics")
def analyze_metrics_prompt():
    """
    Шаблон для анализа маркетинговых метрик.
    """
    return {
        "messages": [
            {
                "role": "system",
                "content": """Ты - эксперт по цифровому маркетингу.
                Анализируй метрики кампаний и давай рекомендации по оптимизации."""
            },
            {
                "role": "user",
                "content": """Проанализируй следующие метрики кампании:
                1. Рассчитай CTR, CPC и Conversion Rate
                2. Оцени эффективность кампании
                3. Дай рекомендации по улучшению

                Используй доступные инструменты для расчета метрик."""
            }
        ]
    }

if __name__ == "__main__":
    logger.info("Starting Marketing Metrics MCP Server...")
    server.run()
```
</details>

---

# МОДУЛЬ 4: PRODUCTION И БЕЗОПАСНОСТЬ

## 🎯 Цели модуля:
- Понять требования безопасности
- Научиться готовить серверы к production
- Узнать best practices

---

## 📖 ТЕОРИЯ (15 минут)

### Безопасность: 5 главных правил

#### 1. Аутентификация и авторизация

**❌ НЕПРАВИЛЬНО:**
```python
@server.tool()
def delete_all_data():
    """Удалить все данные"""
    database.delete_all()  # Опасно!
```

**✅ ПРАВИЛЬНО:**
```python
@server.tool()
def delete_campaign(campaign_id: str, context: AuthContext) -> dict:
    """Удалить кампанию"""
    # Проверка прав доступа
    if context.role != "admin":
        raise PermissionError("Только администраторы могут удалять кампании")

    # Проверка владения
    if not user_owns_campaign(context.user_id, campaign_id):
        raise PermissionError("Вы можете удалять только свои кампании")

    return delete_campaign_from_db(campaign_id)
```

#### 2. Валидация входных данных

**❌ НЕПРАВИЛЬНО:**
```python
@server.tool()
def get_user(user_id: str):
    query = f"SELECT * FROM users WHERE id = {user_id}"  # SQL Injection!
    return db.execute(query)
```

**✅ ПРАВИЛЬНО:**
```python
import re

@server.tool()
def get_user(user_id: str):
    # Валидация формата
    if not re.match(r'^\d{6}$', user_id):
        raise ValueError("User ID должен быть 6-значным числом")

    # Parameterized query
    query = "SELECT * FROM users WHERE id = $1"
    return db.execute(query, user_id)
```

#### 3. Секреты и credentials

**❌ НЕПРАВИЛЬНО:**
```python
API_KEY = "sk-1234567890abcdef"  # Hardcoded!
DATABASE_URL = "postgresql://admin:password@localhost"  # В коде!
```

**✅ ПРАВИЛЬНО:**
```python
import os
from dotenv import load_dotenv

load_dotenv()

API_KEY = os.getenv("API_KEY")
DATABASE_URL = os.getenv("DATABASE_URL")

if not API_KEY:
    raise ValueError("API_KEY не установлен в environment")
```

#### 4. Rate Limiting

```python
from datetime import datetime, timedelta
from collections import defaultdict

class RateLimiter:
    def __init__(self, max_requests: int, time_window: int):
        self.max_requests = max_requests
        self.time_window = time_window
        self.requests = defaultdict(list)

    def is_allowed(self, user_id: str) -> bool:
        now = datetime.now()

        # Удалить старые запросы
        self.requests[user_id] = [
            timestamp for timestamp in self.requests[user_id]
            if now - timestamp < timedelta(seconds=self.time_window)
        ]

        # Проверить лимит
        if len(self.requests[user_id]) < self.max_requests:
            self.requests[user_id].append(now)
            return True

        return False

# Использование
limiter = RateLimiter(max_requests=100, time_window=60)  # 100 запросов в минуту

@server.tool()
def expensive_operation(user_id: str):
    if not limiter.is_allowed(user_id):
        raise Exception("Rate limit exceeded. Попробуйте позже.")

    # Выполнить операцию
    return result
```

#### 5. Логирование (правильное)

```python
import logging
import json
from datetime import datetime

logger = logging.getLogger(__name__)

@server.tool()
def sensitive_operation(user_id: str, data: dict):
    # Структурированное логирование
    log_data = {
        "timestamp": datetime.now().isoformat(),
        "operation": "sensitive_operation",
        "user_id": user_id,
        "ip_address": get_client_ip(),
        "status": "started"
    }

    logger.info(json.dumps(log_data))

    try:
        result = perform_operation(data)
        log_data["status"] = "success"
        logger.info(json.dumps(log_data))
        return result

    except Exception as e:
        log_data["status"] = "error"
        log_data["error"] = str(e)
        logger.error(json.dumps(log_data))
        raise
```

---

## 🧠 КВИЗ 4: Безопасность

### Вопрос 1
**Что такое SQL Injection и как его предотвратить?**

A) Ошибка в SQL синтаксисе
B) Атака через внедрение SQL кода в параметры
C) Медленный SQL запрос
D) Проблема с кодировкой

<details>
<summary>✅ Ответ</summary>

**B) Атака через внедрение SQL кода в параметры**

Объяснение: SQL Injection - это когда злоумышленник может внедрить свой SQL код через параметры запроса. Предотвращается использованием parameterized queries.
</details>

### Вопрос 2
**Где правильно хранить API ключи?**

A) В коде
B) В комментариях
C) В environment variables
D) В README файле

<details>
<summary>✅ Ответ</summary>

**C) В environment variables**

Объяснение: Секреты должны храниться в environment variables (для dev) или в специальных хранилищах секретов (AWS Secrets Manager, HashiCorp Vault для production).
</details>

### Вопрос 3
**Что такое Rate Limiting?**

A) Ограничение размера данных
B) Ограничение количества запросов в единицу времени
C) Ограничение скорости интернета
D) Ограничение числа пользователей

<details>
<summary>✅ Ответ</summary>

**B) Ограничение количества запросов в единицу времени**

Объяснение: Rate Limiting защищает от чрезмерного использования ресурсов, ограничивая количество запросов, которые пользователь может сделать за определенный период.
</details>

---

## 💼 ПРАКТИЧЕСКОЕ ЗАДАНИЕ 5: Найти уязвимости

**Время: 15 минут**

**Задача:** В следующем коде есть **5 серьезных уязвимостей безопасности**. Найдите их все и предложите исправления.

```python
from mcp import Server

server = Server("vulnerable-server")

# API ключ для доступа к внешнему сервису
EXTERNAL_API_KEY = "sk-proj-1234567890abcdef"

@server.tool()
def search_users(query: str) -> list:
    """Поиск пользователей по запросу"""
    print(f"Searching for: {query}")

    # Поиск в базе данных
    sql = f"SELECT * FROM users WHERE name LIKE '%{query}%'"
    results = database.execute(sql)

    return results

@server.tool()
def delete_user(user_id: str) -> dict:
    """Удалить пользователя"""
    database.execute(f"DELETE FROM users WHERE id = {user_id}")
    return {"status": "deleted"}

@server.tool()
def export_all_data(format: str) -> str:
    """Экспортировать все данные"""
    if format == "json":
        data = database.get_all_data()
        return json.dumps(data)
    else:
        return "Invalid format"

if __name__ == "__main__":
    server.run()
```

**Найдите 5 уязвимостей:**

1. **Уязвимость 1:** _______________________________
   **Исправление:** _______________________________

2. **Уязвимость 2:** _______________________________
   **Исправление:** _______________________________

3. **Уязвимость 3:** _______________________________
   **Исправление:** _______________________________

4. **Уязвимость 4:** _______________________________
   **Исправление:** _______________________________

5. **Уязвимость 5:** _______________________________
   **Исправление:** _______________________________

**Правильные ответы:**

<details>
<summary>Показать все уязвимости</summary>

1. **Hardcoded API Key**
   - Уязвимость: API ключ захардкожен в коде
   - Исправление: Использовать `os.getenv("EXTERNAL_API_KEY")`

2. **SQL Injection в search_users**
   - Уязвимость: Запрос формируется конкатенацией строк
   - Исправление: Использовать parameterized query: `"SELECT * FROM users WHERE name LIKE $1", f"%{query}%"`

3. **SQL Injection в delete_user**
   - Уязвимость: User ID вставляется напрямую в запрос
   - Исправление: Использовать parameterized query: `"DELETE FROM users WHERE id = $1", user_id`

4. **Отсутствие авторизации**
   - Уязвимость: Любой может удалять пользователей без проверки прав
   - Исправление: Добавить проверку роли и прав доступа

5. **print() в MCP Server**
   - Уязвимость: print() нарушит JSON-RPC протокол
   - Исправление: Использовать `logger.info(f"Searching for: {query}")`

**Бонусная уязвимость:**
6. **Экспорт всех данных без ограничений**
   - Уязвимость: Любой может экспортировать все данные
   - Исправление: Добавить проверку прав и ограничения по объему данных
</details>

---

## 💼 ПРАКТИЧЕСКОЕ ЗАДАНИЕ 6: Переписать код безопасно

**Время: 15 минут**

**Задача:** Переписать код выше с исправлением всех уязвимостей.

```python
from mcp import Server
from mcp.types import AuthContext
import logging
import os
import re
from dotenv import load_dotenv

load_dotenv()
logger = logging.getLogger(__name__)

server = Server("secure-server")

# ВАШ БЕЗОПАСНЫЙ КОД ЗДЕСЬ
```

---

# 🎓 ФИНАЛЬНЫЙ ТЕСТ

## Проверьте свои знания!

**Пройдите финальный тест, чтобы оценить усвоение материала.**

### Вопрос 1 (5 баллов)
**Объясните своими словами, что такое MCP и какую проблему он решает.**

```
Мой ответ:
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

### Вопрос 2 (5 баллов)
**Перечислите 3 главных компонента MCP и объясните роль каждого.**

```
Мой ответ:
1. _____________________________________________________________
2. _____________________________________________________________
3. _____________________________________________________________
```

### Вопрос 3 (5 баллов)
**Какие 3 примитива предоставляет MCP Server? Чем они отличаются?**

```
Мой ответ:
1. _____________________________________________________________
2. _____________________________________________________________
3. _____________________________________________________________
```

### Вопрос 4 (10 баллов)
**Напишите код простого MCP Server с одним Tool для расчета ROAS (Return on Ad Spend).**

```python
# ВАШ КОД ЗДЕСЬ
```

### Вопрос 5 (10 баллов)
**Перечислите 5 основных правил безопасности при разработке MCP Server.**

```
Мой ответ:
1. _____________________________________________________________
2. _____________________________________________________________
3. _____________________________________________________________
4. _____________________________________________________________
5. _____________________________________________________________
```

### Вопрос 6 (15 баллов)
**Опишите конкретный бизнес-кейс применения MCP для реального клиента/проекта:**
- Индустрия
- Проблема
- Архитектура решения (Host + Servers)
- Ожидаемый ROI

```
Мой ответ:
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

---

## 📊 ОЦЕНКА РЕЗУЛЬТАТОВ

**Подсчитайте баллы:**

- **50-45 баллов:** 🏆 Отлично! Вы готовы разрабатывать и внедрять MCP решения.
- **44-35 баллов:** ✅ Хорошо! Повторите модули 3-4 для укрепления практических навыков.
- **34-25 баллов:** 📚 Удовлетворительно. Пройдите тренинг заново, уделяя больше времени практическим заданиям.
- **Менее 25 баллов:** 🔄 Начните заново с модуля 1 и делайте все задания последовательно.

---

## 🎯 СЛЕДУЮЩИЕ ШАГИ

После завершения тренинга:

1. **Практика:**
   - Клонируйте quickstart examples
   - Создайте свой первый MCP Server
   - Протестируйте с Claude Desktop

2. **Углубление:**
   - Изучите официальную документацию: https://modelcontextprotocol.io
   - Пройдите продвинутые курсы: https://anthropic.skilljar.com

3. **Внедрение:**
   - Выберите реальный проект
   - Спроектируйте архитектуру
   - Начните с MVP (1-2 сервера)

4. **Сообщество:**
   - GitHub: https://github.com/modelcontextprotocol
   - MCP Registry: https://registry.modelcontextprotocol.io

---

**Успехов в изучении MCP! 🚀**
