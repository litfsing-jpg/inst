# 🔧 ГОТОВЫЕ ШАБЛОНЫ КОДА MCP
## Copy-paste решения для быстрого старта

---

## 📋 СОДЕРЖАНИЕ

1. [Базовые шаблоны серверов](#базовые-шаблоны-серверов)
2. [Шаблоны для популярных интеграций](#шаблоны-для-популярных-интеграций)
3. [Шаблоны безопасности](#шаблоны-безопасности)
4. [Конфигурационные файлы](#конфигурационные-файлы)
5. [Docker и деплой](#docker-и-деплой)

---

# БАЗОВЫЕ ШАБЛОНЫ СЕРВЕРОВ

## 🎯 Шаблон 1: Минимальный MCP Server

```python
"""
Минимальный рабочий MCP Server
"""
from mcp import Server
from mcp.types import TextContent, Tool
import logging
import sys

# Настройка логирования (только в stderr!)
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    stream=sys.stderr  # ВАЖНО: только stderr!
)
logger = logging.getLogger(__name__)

# Создание сервера
server = Server("my-mcp-server")

@server.tool()
def hello_world(name: str) -> dict:
    """
    Приветствие пользователя.

    Args:
        name: Имя пользователя

    Returns:
        Приветственное сообщение
    """
    logger.info(f"Greeting user: {name}")

    return {
        "message": f"Hello, {name}!",
        "timestamp": "2025-12-03"
    }

@server.resource("info://version")
def get_version() -> str:
    """Версия сервера"""
    return "1.0.0"

if __name__ == "__main__":
    logger.info("Starting MCP Server...")
    server.run()
```

---

## 🎯 Шаблон 2: Полнофункциональный MCP Server

```python
"""
Полнофункциональный MCP Server с Tools, Resources и Prompts
"""
from mcp import Server
from mcp.types import TextContent, Tool, Prompt
import logging
import sys
import os
from typing import Optional
from dotenv import load_dotenv

# Загрузка переменных окружения
load_dotenv()

# Настройка логирования
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    stream=sys.stderr
)
logger = logging.getLogger(__name__)

# Создание сервера
server = Server("advanced-mcp-server")

# === TOOLS ===

@server.tool()
def process_data(
    data: str,
    operation: str = "uppercase"
) -> dict:
    """
    Обработка данных с различными операциями.

    Args:
        data: Входные данные для обработки
        operation: Тип операции (uppercase, lowercase, reverse)

    Returns:
        Результат обработки
    """
    logger.info(f"Processing data with operation: {operation}")

    try:
        if operation == "uppercase":
            result = data.upper()
        elif operation == "lowercase":
            result = data.lower()
        elif operation == "reverse":
            result = data[::-1]
        else:
            return {
                "success": False,
                "error": f"Unknown operation: {operation}"
            }

        return {
            "success": True,
            "result": result,
            "operation": operation
        }

    except Exception as e:
        logger.error(f"Error processing data: {e}")
        return {
            "success": False,
            "error": str(e)
        }

@server.tool()
def validate_input(text: str, min_length: int = 1) -> dict:
    """
    Валидация входных данных.

    Args:
        text: Текст для валидации
        min_length: Минимальная длина текста

    Returns:
        Результат валидации
    """
    is_valid = len(text) >= min_length

    return {
        "valid": is_valid,
        "length": len(text),
        "min_length": min_length
    }

# === RESOURCES ===

@server.resource("config://settings")
def get_settings() -> str:
    """Настройки сервера"""
    return """
    Server Settings:
    - Version: 1.0.0
    - Environment: Production
    - Max connections: 100
    """

@server.resource("docs://api")
def get_api_docs() -> str:
    """API документация"""
    return """
    API Documentation

    Tools:
    1. process_data(data, operation)
       - Обработка данных
       - Operations: uppercase, lowercase, reverse

    2. validate_input(text, min_length)
       - Валидация входных данных
    """

# === PROMPTS ===

@server.prompt("analyze-text")
def analyze_text_prompt() -> dict:
    """Шаблон для анализа текста"""
    return {
        "messages": [
            {
                "role": "system",
                "content": "Ты - эксперт по анализу текста."
            },
            {
                "role": "user",
                "content": """
                Проанализируй следующий текст:
                1. Определи основную тему
                2. Выдели ключевые слова
                3. Оцени тональность (позитив/негатив/нейтрал)
                """
            }
        ]
    }

# Запуск сервера
if __name__ == "__main__":
    logger.info("Starting Advanced MCP Server...")
    try:
        server.run()
    except Exception as e:
        logger.error(f"Server error: {e}")
        sys.exit(1)
```

---

# ШАБЛОНЫ ДЛЯ ПОПУЛЯРНЫХ ИНТЕГРАЦИЙ

## 📊 Шаблон 3: Интеграция с PostgreSQL

```python
"""
MCP Server для работы с PostgreSQL
"""
from mcp import Server
import psycopg2
from psycopg2 import pool
import logging
import sys
import os
from typing import List, Dict, Any
from dotenv import load_dotenv

load_dotenv()

logging.basicConfig(level=logging.INFO, stream=sys.stderr)
logger = logging.getLogger(__name__)

server = Server("postgresql-mcp")

# Connection pool для эффективной работы
connection_pool = None

def init_connection_pool():
    """Инициализация пула соединений"""
    global connection_pool
    try:
        connection_pool = psycopg2.pool.SimpleConnectionPool(
            minconn=1,
            maxconn=10,
            host=os.getenv("DB_HOST"),
            database=os.getenv("DB_NAME"),
            user=os.getenv("DB_USER"),
            password=os.getenv("DB_PASSWORD"),
            port=os.getenv("DB_PORT", "5432")
        )
        logger.info("Database connection pool initialized")
    except Exception as e:
        logger.error(f"Failed to initialize connection pool: {e}")
        raise

@server.tool()
def execute_query(query: str, params: List[Any] = None) -> dict:
    """
    Выполнить SQL запрос (SELECT).

    Args:
        query: SQL запрос
        params: Параметры запроса

    Returns:
        Результаты запроса
    """
    logger.info(f"Executing query: {query[:100]}...")

    # Валидация: только SELECT запросы
    if not query.strip().upper().startswith("SELECT"):
        return {
            "success": False,
            "error": "Only SELECT queries are allowed"
        }

    conn = None
    try:
        conn = connection_pool.getconn()
        cursor = conn.cursor()

        # Parameterized query для защиты от SQL injection
        cursor.execute(query, params or [])
        columns = [desc[0] for desc in cursor.description]
        rows = cursor.fetchall()

        results = [dict(zip(columns, row)) for row in rows]

        cursor.close()

        return {
            "success": True,
            "row_count": len(results),
            "results": results
        }

    except Exception as e:
        logger.error(f"Query error: {e}")
        return {
            "success": False,
            "error": str(e)
        }

    finally:
        if conn:
            connection_pool.putconn(conn)

@server.tool()
def get_table_schema(table_name: str) -> dict:
    """
    Получить схему таблицы.

    Args:
        table_name: Название таблицы

    Returns:
        Схема таблицы
    """
    query = """
        SELECT column_name, data_type, is_nullable
        FROM information_schema.columns
        WHERE table_name = %s
        ORDER BY ordinal_position
    """

    return execute_query(query, [table_name])

@server.resource("database://schema")
def get_database_schema() -> str:
    """Полная схема базы данных"""
    conn = None
    try:
        conn = connection_pool.getconn()
        cursor = conn.cursor()

        cursor.execute("""
            SELECT table_name
            FROM information_schema.tables
            WHERE table_schema = 'public'
        """)

        tables = cursor.fetchall()
        schema = "Database Schema:\n\n"

        for (table_name,) in tables:
            schema += f"Table: {table_name}\n"
            cursor.execute("""
                SELECT column_name, data_type
                FROM information_schema.columns
                WHERE table_name = %s
            """, (table_name,))

            columns = cursor.fetchall()
            for col_name, data_type in columns:
                schema += f"  - {col_name}: {data_type}\n"
            schema += "\n"

        cursor.close()
        return schema

    except Exception as e:
        logger.error(f"Error getting schema: {e}")
        return f"Error: {str(e)}"

    finally:
        if conn:
            connection_pool.putconn(conn)

if __name__ == "__main__":
    init_connection_pool()
    logger.info("Starting PostgreSQL MCP Server...")
    server.run()
```

---

## 📧 Шаблон 4: Интеграция с Email (SMTP)

```python
"""
MCP Server для отправки email
"""
from mcp import Server
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
import logging
import sys
import os
from typing import List, Optional
from dotenv import load_dotenv

load_dotenv()

logging.basicConfig(level=logging.INFO, stream=sys.stderr)
logger = logging.getLogger(__name__)

server = Server("email-mcp")

def send_email_smtp(
    to: str,
    subject: str,
    body: str,
    html: bool = False
) -> bool:
    """Отправка email через SMTP"""
    try:
        msg = MIMEMultipart('alternative')
        msg['From'] = os.getenv("SMTP_FROM")
        msg['To'] = to
        msg['Subject'] = subject

        if html:
            msg.attach(MIMEText(body, 'html'))
        else:
            msg.attach(MIMEText(body, 'plain'))

        with smtplib.SMTP(
            os.getenv("SMTP_HOST"),
            int(os.getenv("SMTP_PORT", "587"))
        ) as server:
            server.starttls()
            server.login(
                os.getenv("SMTP_USER"),
                os.getenv("SMTP_PASSWORD")
            )
            server.send_message(msg)

        return True

    except Exception as e:
        logger.error(f"Failed to send email: {e}")
        return False

@server.tool()
def send_email(
    to: str,
    subject: str,
    body: str,
    html: bool = False
) -> dict:
    """
    Отправить email.

    Args:
        to: Email получателя
        subject: Тема письма
        body: Текст письма
        html: Использовать HTML формат

    Returns:
        Результат отправки
    """
    logger.info(f"Sending email to {to}")

    # Валидация email
    if "@" not in to or "." not in to:
        return {
            "success": False,
            "error": "Invalid email address"
        }

    success = send_email_smtp(to, subject, body, html)

    return {
        "success": success,
        "to": to,
        "subject": subject
    }

@server.tool()
def send_bulk_email(
    recipients: List[str],
    subject: str,
    body: str
) -> dict:
    """
    Отправить email нескольким получателям.

    Args:
        recipients: Список email получателей
        subject: Тема письма
        body: Текст письма

    Returns:
        Результаты отправки
    """
    logger.info(f"Sending bulk email to {len(recipients)} recipients")

    results = {
        "sent": 0,
        "failed": 0,
        "details": []
    }

    for recipient in recipients:
        success = send_email_smtp(recipient, subject, body)

        if success:
            results["sent"] += 1
        else:
            results["failed"] += 1

        results["details"].append({
            "email": recipient,
            "success": success
        })

    return results

@server.prompt("compose-marketing-email")
def compose_marketing_email_prompt() -> dict:
    """Шаблон для создания маркетингового email"""
    return {
        "messages": [
            {
                "role": "system",
                "content": "Ты - эксперт по email маркетингу."
            },
            {
                "role": "user",
                "content": """
                Создай маркетинговое письмо:
                1. Привлекающая тема (не более 50 символов)
                2. Персонализированное приветствие
                3. Ценностное предложение
                4. Четкий call-to-action
                5. PS с дополнительной мотивацией
                """
            }
        ]
    }

if __name__ == "__main__":
    logger.info("Starting Email MCP Server...")
    server.run()
```

---

## 🌐 Шаблон 5: Интеграция с REST API

```python
"""
MCP Server для работы с внешним REST API
"""
from mcp import Server
import requests
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry
import logging
import sys
import os
from typing import Dict, Any, Optional
from dotenv import load_dotenv

load_dotenv()

logging.basicConfig(level=logging.INFO, stream=sys.stderr)
logger = logging.getLogger(__name__)

server = Server("api-mcp")

# Настройка HTTP сессии с retry логикой
session = requests.Session()
retry_strategy = Retry(
    total=3,
    backoff_factor=1,
    status_forcelist=[429, 500, 502, 503, 504]
)
adapter = HTTPAdapter(max_retries=retry_strategy)
session.mount("http://", adapter)
session.mount("https://", adapter)

# Базовый URL и headers
BASE_URL = os.getenv("API_BASE_URL")
API_KEY = os.getenv("API_KEY")

def get_headers() -> Dict[str, str]:
    """Получить headers для запросов"""
    return {
        "Authorization": f"Bearer {API_KEY}",
        "Content-Type": "application/json"
    }

@server.tool()
def api_get(endpoint: str, params: Optional[Dict] = None) -> dict:
    """
    GET запрос к API.

    Args:
        endpoint: API endpoint (без базового URL)
        params: Query параметры

    Returns:
        Ответ от API
    """
    url = f"{BASE_URL}/{endpoint.lstrip('/')}"
    logger.info(f"GET request to {url}")

    try:
        response = session.get(
            url,
            headers=get_headers(),
            params=params or {},
            timeout=30
        )
        response.raise_for_status()

        return {
            "success": True,
            "status_code": response.status_code,
            "data": response.json()
        }

    except requests.exceptions.RequestException as e:
        logger.error(f"API request failed: {e}")
        return {
            "success": False,
            "error": str(e)
        }

@server.tool()
def api_post(endpoint: str, data: Dict[str, Any]) -> dict:
    """
    POST запрос к API.

    Args:
        endpoint: API endpoint
        data: Данные для отправки

    Returns:
        Ответ от API
    """
    url = f"{BASE_URL}/{endpoint.lstrip('/')}"
    logger.info(f"POST request to {url}")

    try:
        response = session.post(
            url,
            headers=get_headers(),
            json=data,
            timeout=30
        )
        response.raise_for_status()

        return {
            "success": True,
            "status_code": response.status_code,
            "data": response.json()
        }

    except requests.exceptions.RequestException as e:
        logger.error(f"API request failed: {e}")
        return {
            "success": False,
            "error": str(e)
        }

@server.resource("api://endpoints")
def get_api_endpoints() -> str:
    """Список доступных API endpoints"""
    return """
    Available API Endpoints:

    GET /users - Get all users
    GET /users/{id} - Get user by ID
    POST /users - Create new user
    GET /products - Get all products
    POST /orders - Create new order
    """

if __name__ == "__main__":
    logger.info("Starting API MCP Server...")
    server.run()
```

---

# ШАБЛОНЫ БЕЗОПАСНОСТИ

## 🔒 Шаблон 6: Сервер с аутентификацией и авторизацией

```python
"""
MCP Server с полной системой безопасности
"""
from mcp import Server
from typing import Optional, Dict, Any
import logging
import sys
import os
import re
from functools import wraps
from datetime import datetime, timedelta
import jwt
from dotenv import load_dotenv

load_dotenv()

logging.basicConfig(level=logging.INFO, stream=sys.stderr)
logger = logging.getLogger(__name__)

server = Server("secure-mcp")

# Секретный ключ для JWT
JWT_SECRET = os.getenv("JWT_SECRET")
JWT_ALGORITHM = "HS256"

# === AUTH CONTEXT ===

class AuthContext:
    """Контекст аутентификации"""

    def __init__(self, user_id: str, role: str, permissions: list):
        self.user_id = user_id
        self.role = role
        self.permissions = permissions

    @classmethod
    def from_token(cls, token: str) -> Optional['AuthContext']:
        """Создать контекст из JWT токена"""
        try:
            payload = jwt.decode(token, JWT_SECRET, algorithms=[JWT_ALGORITHM])
            return cls(
                user_id=payload['user_id'],
                role=payload['role'],
                permissions=payload.get('permissions', [])
            )
        except jwt.InvalidTokenError as e:
            logger.error(f"Invalid token: {e}")
            return None

# === ДЕКОРАТОРЫ ДЛЯ ПРОВЕРКИ ПРАВ ===

def require_auth(func):
    """Декоратор для проверки аутентификации"""
    @wraps(func)
    def wrapper(*args, **kwargs):
        # Здесь должна быть логика получения токена
        # Для примера используем заглушку
        context = kwargs.get('context')

        if not context:
            return {
                "success": False,
                "error": "Authentication required"
            }

        return func(*args, **kwargs)
    return wrapper

def require_role(allowed_roles: list):
    """Декоратор для проверки роли"""
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            context = kwargs.get('context')

            if not context or context.role not in allowed_roles:
                return {
                    "success": False,
                    "error": f"Required role: {', '.join(allowed_roles)}"
                }

            return func(*args, **kwargs)
        return wrapper
    return decorator

# === ВАЛИДАЦИЯ ДАННЫХ ===

def validate_email(email: str) -> bool:
    """Валидация email адреса"""
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    return re.match(pattern, email) is not None

def validate_user_id(user_id: str) -> bool:
    """Валидация ID пользователя"""
    return re.match(r'^\d{6}$', user_id) is not None

def sanitize_input(text: str) -> str:
    """Очистка входных данных от опасных символов"""
    # Удаляем потенциально опасные символы
    dangerous_chars = ['<', '>', '"', "'", ';', '&', '|']
    for char in dangerous_chars:
        text = text.replace(char, '')
    return text.strip()

# === RATE LIMITING ===

class RateLimiter:
    """Простой rate limiter"""

    def __init__(self, max_requests: int, time_window: int):
        self.max_requests = max_requests
        self.time_window = time_window
        self.requests = {}

    def is_allowed(self, user_id: str) -> bool:
        """Проверка лимита запросов"""
        now = datetime.now()

        if user_id not in self.requests:
            self.requests[user_id] = []

        # Удаляем старые запросы
        self.requests[user_id] = [
            timestamp for timestamp in self.requests[user_id]
            if now - timestamp < timedelta(seconds=self.time_window)
        ]

        # Проверяем лимит
        if len(self.requests[user_id]) < self.max_requests:
            self.requests[user_id].append(now)
            return True

        return False

rate_limiter = RateLimiter(max_requests=100, time_window=60)

# === SECURE TOOLS ===

@server.tool()
def get_user_data(user_id: str, context: AuthContext = None) -> dict:
    """
    Получить данные пользователя (требует аутентификации).

    Args:
        user_id: ID пользователя
        context: Контекст аутентификации

    Returns:
        Данные пользователя
    """
    logger.info(f"Getting data for user: {user_id}")

    # Проверка аутентификации
    if not context:
        return {
            "success": False,
            "error": "Authentication required"
        }

    # Валидация user_id
    if not validate_user_id(user_id):
        return {
            "success": False,
            "error": "Invalid user ID format"
        }

    # Проверка прав доступа (пользователь может видеть только свои данные)
    if context.user_id != user_id and context.role != "admin":
        return {
            "success": False,
            "error": "Access denied: Can only view your own data"
        }

    # Rate limiting
    if not rate_limiter.is_allowed(context.user_id):
        return {
            "success": False,
            "error": "Rate limit exceeded"
        }

    # Здесь должна быть логика получения данных из БД
    # Для примера возвращаем заглушку
    return {
        "success": True,
        "user_id": user_id,
        "name": "John Doe",
        "email": "john@example.com"
    }

@server.tool()
def delete_user(user_id: str, context: AuthContext = None) -> dict:
    """
    Удалить пользователя (только для админов).

    Args:
        user_id: ID пользователя
        context: Контекст аутентификации

    Returns:
        Результат операции
    """
    logger.info(f"Delete request for user: {user_id}")

    # Проверка аутентификации
    if not context:
        return {
            "success": False,
            "error": "Authentication required"
        }

    # Проверка роли (только админы)
    if context.role != "admin":
        return {
            "success": False,
            "error": "Access denied: Admin role required"
        }

    # Валидация
    if not validate_user_id(user_id):
        return {
            "success": False,
            "error": "Invalid user ID format"
        }

    # Логирование критичной операции
    logger.warning(
        f"User deletion: user_id={user_id}, "
        f"by_admin={context.user_id}"
    )

    # Здесь должна быть логика удаления из БД
    return {
        "success": True,
        "user_id": user_id,
        "deleted_at": datetime.now().isoformat()
    }

if __name__ == "__main__":
    logger.info("Starting Secure MCP Server...")
    server.run()
```

---

# КОНФИГУРАЦИОННЫЕ ФАЙЛЫ

## ⚙️ Шаблон 7: .env файл

```bash
# === DATABASE ===
DB_HOST=localhost
DB_PORT=5432
DB_NAME=myapp
DB_USER=postgres
DB_PASSWORD=your_secure_password

# === API KEYS ===
API_KEY=your_api_key_here
JWT_SECRET=your_jwt_secret_here

# === EMAIL (SMTP) ===
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASSWORD=your_app_password
SMTP_FROM=noreply@yourdomain.com

# === EXTERNAL APIS ===
API_BASE_URL=https://api.example.com/v1
YANDEX_DIRECT_TOKEN=your_yandex_token
GOOGLE_ADS_CLIENT_ID=your_google_client_id

# === APPLICATION ===
ENVIRONMENT=production
LOG_LEVEL=INFO
MAX_CONNECTIONS=100

# === RATE LIMITING ===
RATE_LIMIT_REQUESTS=100
RATE_LIMIT_WINDOW=60
```

---

## ⚙️ Шаблон 8: Claude Desktop Config

```json
{
  "mcpServers": {
    "my-custom-server": {
      "command": "python",
      "args": ["/path/to/your/server.py"],
      "env": {
        "API_KEY": "your_api_key"
      }
    },
    "postgresql-server": {
      "command": "python",
      "args": ["/path/to/postgresql_server.py"]
    },
    "email-server": {
      "command": "python",
      "args": ["/path/to/email_server.py"]
    }
  }
}
```

**Путь к файлу конфигурации:**
- **MacOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
- **Linux:** `~/.config/Claude/claude_desktop_config.json`

---

# DOCKER И ДЕПЛОЙ

## 🐳 Шаблон 9: Dockerfile

```dockerfile
# Используем официальный Python образ
FROM python:3.11-slim

# Устанавливаем рабочую директорию
WORKDIR /app

# Копируем requirements
COPY requirements.txt .

# Устанавливаем зависимости
RUN pip install --no-cache-dir -r requirements.txt

# Копируем код приложения
COPY . .

# Создаем non-root пользователя для безопасности
RUN useradd -m -u 1000 appuser && chown -R appuser:appuser /app
USER appuser

# Healthcheck
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD python -c "import sys; sys.exit(0)"

# Запуск сервера
CMD ["python", "server.py"]
```

---

## 🐳 Шаблон 10: docker-compose.yml

```yaml
version: '3.8'

services:
  mcp-server:
    build: .
    container_name: mcp-server
    restart: unless-stopped
    ports:
      - "8000:8000"
    environment:
      - DB_HOST=postgres
      - DB_PORT=5432
      - DB_NAME=myapp
      - DB_USER=postgres
      - DB_PASSWORD=${DB_PASSWORD}
      - API_KEY=${API_KEY}
      - LOG_LEVEL=INFO
    volumes:
      - ./logs:/app/logs
    depends_on:
      - postgres
      - redis
    networks:
      - mcp-network

  postgres:
    image: postgres:15-alpine
    container_name: mcp-postgres
    restart: unless-stopped
    environment:
      - POSTGRES_DB=myapp
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=${DB_PASSWORD}
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - mcp-network

  redis:
    image: redis:7-alpine
    container_name: mcp-redis
    restart: unless-stopped
    networks:
      - mcp-network

volumes:
  postgres-data:

networks:
  mcp-network:
    driver: bridge
```

---

## 🐳 Шаблон 11: requirements.txt

```txt
# MCP SDK
mcp==0.1.0

# Database
psycopg2-binary==2.9.9
sqlalchemy==2.0.23

# HTTP клиент
requests==2.31.0
httpx==0.25.2

# Environment variables
python-dotenv==1.0.0

# Auth
pyjwt==2.8.0
cryptography==41.0.7

# Async
asyncio==3.4.3
aiohttp==3.9.1

# Redis (для кеширования)
redis==5.0.1

# Мониторинг
prometheus-client==0.19.0

# Тестирование
pytest==7.4.3
pytest-asyncio==0.21.1
```

---

# 📝 ШАБЛОН ДОКУМЕНТАЦИИ

## Шаблон 12: README.md для MCP Server

```markdown
# My MCP Server

Описание того, что делает ваш MCP Server.

## Возможности

- ✅ Интеграция с [система 1]
- ✅ Интеграция с [система 2]
- ✅ Аутентификация и авторизация
- ✅ Rate limiting
- ✅ Structured logging

## Требования

- Python 3.9+
- PostgreSQL 15+ (опционально)
- Redis (опционально)

## Установка

### 1. Клонирование репозитория

```bash
git clone https://github.com/yourname/your-mcp-server.git
cd your-mcp-server
```

### 2. Установка зависимостей

```bash
pip install -r requirements.txt
```

### 3. Настройка environment variables

Создайте `.env` файл:

```bash
cp .env.example .env
```

Заполните необходимые переменные в `.env`.

### 4. Запуск сервера

```bash
python server.py
```

## Конфигурация Claude Desktop

Добавьте в `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "my-server": {
      "command": "python",
      "args": ["/absolute/path/to/server.py"]
    }
  }
}
```

## API Reference

### Tools

#### `tool_name(param1, param2)`

Описание инструмента.

**Параметры:**
- `param1` (string): Описание параметра 1
- `param2` (int): Описание параметра 2

**Возвращает:**
```json
{
  "success": true,
  "data": {...}
}
```

### Resources

#### `resource://name`

Описание ресурса.

### Prompts

#### `prompt-name`

Описание промпта.

## Безопасность

- ✅ Валидация всех входных данных
- ✅ Parameterized queries (защита от SQL injection)
- ✅ Rate limiting
- ✅ Secrets в environment variables

## Development

### Запуск тестов

```bash
pytest tests/
```

### Линтинг

```bash
flake8 .
black .
```

## Деплой

### Docker

```bash
docker build -t my-mcp-server .
docker run -p 8000:8000 my-mcp-server
```

### Docker Compose

```bash
docker-compose up -d
```

## Мониторинг

Метрики доступны на `/metrics` (Prometheus format).

## Troubleshooting

### Проблема 1

**Симптомы:** ...
**Решение:** ...

## License

MIT

## Контакты

- Email: your@email.com
- GitHub: @yourname
```

---

**Используйте эти шаблоны как основу для своих MCP серверов! 🚀**

*Адаптируйте код под свои потребности и всегда следуйте best practices безопасности.*
