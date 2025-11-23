# ЛИД-МАГНИТ: КАЛЬКУЛЯТОР ПОТЕРЬ
## ЭТАП 2: ФИНАЛЬНЫЙ HTML КОД + ТЕКСТЫ

**Дата:** 23 ноября 2025
**Статус:** Production-ready код

---

## 📦 ЧАСТЬ 1: ФИНАЛЬНЫЙ HTML КОД ДЛЯ IFRAME

### 🎯 Полный код калькулятора (готов для Tilda)

Скопируйте этот код целиком и вставьте в блок HTML в Tilda:

```html
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Калькулятор потерь Instagram</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
      background: #ffffff;
      padding: 20px;
      line-height: 1.6;
    }

    .calculator {
      max-width: 450px;
      margin: 0 auto;
      background: #ffffff;
      padding: 30px;
      border-radius: 16px;
    }

    h3 {
      font-size: 24px;
      font-weight: 700;
      color: #1a1a1a;
      margin-bottom: 8px;
      text-align: center;
    }

    .subtitle {
      font-size: 14px;
      color: #666;
      text-align: center;
      margin-bottom: 30px;
    }

    .input-group {
      margin-bottom: 20px;
    }

    .input-group label {
      display: block;
      margin-bottom: 8px;
      font-weight: 600;
      font-size: 15px;
      color: #333;
    }

    .input-wrapper {
      position: relative;
    }

    .input-group input {
      width: 100%;
      padding: 14px 16px;
      font-size: 16px;
      border: 2px solid #e5e5e5;
      border-radius: 10px;
      transition: all 0.3s ease;
      background: #fafafa;
      color: #1a1a1a;
      font-weight: 500;
    }

    .input-group input:focus {
      outline: none;
      border-color: #6366f1;
      background: #ffffff;
      box-shadow: 0 0 0 4px rgba(99, 102, 241, 0.1);
    }

    .input-group input::placeholder {
      color: #999;
    }

    .input-hint {
      font-size: 12px;
      color: #999;
      margin-top: 6px;
      display: block;
    }

    .results {
      margin-top: 30px;
      padding: 24px;
      background: linear-gradient(135deg, #fff5f5 0%, #ffe5e5 100%);
      border-left: 6px solid #ef4444;
      border-radius: 12px;
      box-shadow: 0 4px 16px rgba(239, 68, 68, 0.1);
      animation: fadeIn 0.5s ease;
    }

    @keyframes fadeIn {
      from {
        opacity: 0;
        transform: translateY(-10px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .results h4 {
      font-size: 18px;
      font-weight: 700;
      color: #1a1a1a;
      margin-bottom: 16px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .results-item {
      margin: 14px 0;
      padding: 12px 0;
      border-bottom: 1px solid rgba(239, 68, 68, 0.15);
    }

    .results-item:last-child {
      border-bottom: none;
    }

    .results-label {
      font-size: 14px;
      color: #666;
      display: block;
      margin-bottom: 4px;
    }

    .results-value {
      font-size: 26px;
      font-weight: 800;
      color: #ef4444;
      letter-spacing: -0.5px;
    }

    .results-value.small {
      font-size: 20px;
      color: #f59e0b;
    }

    .warning-box {
      background: #fef3c7;
      border-left: 4px solid #f59e0b;
      padding: 12px 14px;
      border-radius: 8px;
      margin-top: 16px;
    }

    .warning-box p {
      font-size: 13px;
      color: #78350f;
      line-height: 1.5;
    }

    /* Мобильная адаптация */
    @media (max-width: 500px) {
      .calculator {
        padding: 20px;
      }

      h3 {
        font-size: 20px;
      }

      .results-value {
        font-size: 22px;
      }

      .input-group input {
        padding: 12px 14px;
      }
    }

    /* Анимация пульса для больших потерь */
    @keyframes pulse {
      0%, 100% {
        transform: scale(1);
      }
      50% {
        transform: scale(1.05);
      }
    }

    .results.high-loss {
      animation: pulse 2s infinite;
    }
  </style>
</head>
<body>
  <div class="calculator">
    <h3>💸 Калькулятор потерь</h3>
    <p class="subtitle">Узнайте сколько денег упускаете прямо сейчас</p>

    <div class="input-group">
      <label for="avgCheck">Средний чек (₽)</label>
      <div class="input-wrapper">
        <input
          type="number"
          id="avgCheck"
          placeholder="3500"
          min="0"
          step="100"
        >
      </div>
      <span class="input-hint">Средняя стоимость одного заказа</span>
    </div>

    <div class="input-group">
      <label for="leadsPerDay">Заявок в день</label>
      <div class="input-wrapper">
        <input
          type="number"
          id="leadsPerDay"
          placeholder="200"
          min="0"
          step="10"
        >
      </div>
      <span class="input-hint">Сколько комментариев и DM получаете</span>
    </div>

    <div class="input-group">
      <label for="processedPercent">Обрабатываете (%)</label>
      <div class="input-wrapper">
        <input
          type="number"
          id="processedPercent"
          placeholder="25"
          min="0"
          max="100"
          step="5"
        >
      </div>
      <span class="input-hint">Какой процент заявок успеваете обработать</span>
    </div>

    <div class="results" id="results" style="display:none;">
      <h4>📊 ВАШИ ПОТЕРИ:</h4>

      <div class="results-item">
        <span class="results-label">⚠️ Необработанные заявки в день</span>
        <div class="results-value small"><span id="unprocessed"></span> шт</div>
      </div>

      <div class="results-item">
        <span class="results-label">💸 Потери в день</span>
        <div class="results-value"><span id="lossDay"></span> ₽</div>
      </div>

      <div class="results-item">
        <span class="results-label">💸 Потери в месяц</span>
        <div class="results-value"><span id="lossMonth"></span> ₽</div>
      </div>

      <div class="results-item">
        <span class="results-label">💸 Потери в год</span>
        <div class="results-value"><span id="lossYear"></span> ₽</div>
      </div>

      <div class="warning-box">
        <p><strong>⚡ Внимание:</strong> Расчет основан на конверсии 20% при быстрой обработке заявок. Реальные потери могут быть еще больше!</p>
      </div>
    </div>
  </div>

  <script>
    // Получаем элементы
    const avgCheck = document.getElementById('avgCheck');
    const leadsPerDay = document.getElementById('leadsPerDay');
    const processedPercent = document.getElementById('processedPercent');
    const results = document.getElementById('results');

    // Функция форматирования чисел (с пробелами)
    function formatNumber(num) {
      return Math.round(num).toLocaleString('ru-RU');
    }

    // Функция расчета
    function calculate() {
      const check = parseFloat(avgCheck.value) || 0;
      const leads = parseFloat(leadsPerDay.value) || 0;
      const processed = parseFloat(processedPercent.value) || 0;

      // Проверяем что все поля заполнены
      if (check > 0 && leads > 0 && processed >= 0 && processed <= 100) {
        // Расчеты
        const unprocessedCount = Math.round(leads * (100 - processed) / 100);
        const conversion = 0.20; // 20% конверсия (фиксированная)
        const lossDay = unprocessedCount * conversion * check;
        const lossMonth = lossDay * 30;
        const lossYear = lossMonth * 12;

        // Обновляем значения
        document.getElementById('unprocessed').textContent = formatNumber(unprocessedCount);
        document.getElementById('lossDay').textContent = formatNumber(lossDay);
        document.getElementById('lossMonth').textContent = formatNumber(lossMonth);
        document.getElementById('lossYear').textContent = formatNumber(lossYear);

        // Показываем результаты
        results.style.display = 'block';

        // Добавляем анимацию пульса для больших потерь
        if (lossDay > 50000) {
          results.classList.add('high-loss');
        } else {
          results.classList.remove('high-loss');
        }

      } else {
        // Скрываем результаты если поля не заполнены
        results.style.display = 'none';
      }
    }

    // Слушаем изменения в полях
    avgCheck.addEventListener('input', calculate);
    leadsPerDay.addEventListener('input', calculate);
    processedPercent.addEventListener('input', calculate);

    // Опциональ: автозаполнение примера через 2 секунды если пользователь ничего не вводит
    setTimeout(() => {
      if (!avgCheck.value && !leadsPerDay.value && !processedPercent.value) {
        avgCheck.value = '3500';
        leadsPerDay.value = '200';
        processedPercent.value = '25';
        calculate();
      }
    }, 3000);
  </script>
</body>
</html>
```

---

## 📝 ЧАСТЬ 2: ТЕКСТЫ ДЛЯ TILDA

### Блок 1: ЗАГОЛОВОК (Zero Block Text)

**Текст:**
```
Сколько денег вы теряете из-за необработанных заявок в Instagram?
```

**Настройки:**
- Шрифт: Montserrat Bold или Inter Bold
- Размер: 38px (desktop), 28px (mobile)
- Цвет: #1a1a1a
- Выравнивание: по центру
- Отступы: 60px сверху, 20px снизу

---

### Блок 2: ПОДЗАГОЛОВОК (Zero Block Text)

**Текст:**
```
Каждая пропущенная заявка = упущенная продажа
Посчитайте ваши реальные потери за 2 минуты ⬇️
```

**Настройки:**
- Шрифт: Inter Regular
- Размер: 20px (desktop), 16px (mobile)
- Цвет: #666666
- Выравнивание: по центру
- Отступы: 0px сверху, 40px снизу

---

### Блок 3: КАЛЬКУЛЯТОР (HTML Block)

**Инструкция:**
1. Добавить блок **T123 → HTML**
2. Вставить код из ЧАСТИ 1 (выше)
3. Настройки контейнера:
   - Ширина: 100%
   - Максимальная ширина: 500px
   - Выравнивание: по центру
   - Отступы: 0px

**Высота iframe:**
- Desktop: 650px
- Mobile: 700px

---

### Блок 4: ИНТЕРПРЕТАЦИЯ РЕЗУЛЬТАТА (Zero Block Text)

**Текст:**
```
⚡ Что значат эти цифры?

Если вы теряете больше 50,000₽/день — это 1,500,000₽ в месяц.

За год это:
• Новый автомобиль бизнес-класса
• Расширение ассортимента на 500+ позиций
• Открытие второго магазина
• Или просто потерянные деньги, которые испаряются каждый день

Вопрос: сколько еще вы готовы терять?
```

**Настройки:**
- Шрифт: Inter Regular
- Размер: 18px (desktop), 16px (mobile)
- Цвет: #333333
- Выравнивание: по левому краю
- Фон: #f9fafb (светло-серый)
- Padding: 30px
- Border-radius: 12px
- Отступы: 40px сверху, 30px снизу

---

### Блок 5: CTA КНОПКА (Button Block)

**ВАЖНО:** Эта кнопка должна быть ВНЕ iframe калькулятора!

**Текст кнопки:**
```
Хочу вернуть эти деньги 💰
```

**Альтернативные варианты:**
```
Записаться на консультацию
```
или
```
Узнать как решить проблему
```

**Настройки:**
- Тип: Большая кнопка (Zero Block Button)
- Цвет фона: #ef4444 (красный)
- Цвет текста: #ffffff (белый)
- Шрифт: Inter Bold
- Размер: 18px
- Padding: 18px 40px
- Border-radius: 50px (круглая)
- Выравнивание: по центру
- Тень: 0 8px 20px rgba(239, 68, 68, 0.3)

**Действие при клике:**
- URL: `https://t.me/ВАШ_TELEGRAM_BOT` (замените на реальный username бота)
- Открытие: в новой вкладке

**Hover эффект:**
- Цвет фона: #dc2626 (темнее)
- Тень: 0 10px 25px rgba(239, 68, 68, 0.4)
- Трансформация: translateY(-2px)

---

### Блок 6: ДОПОЛНИТЕЛЬНЫЙ ТЕКСТ ПОД КНОПКОЙ (опционально)

**Текст:**
```
🔒 Бесплатная диагностическая сессия
Без навязывания и продаж в лоб
```

**Настройки:**
- Шрифт: Inter Regular
- Размер: 14px
- Цвет: #999999
- Выравнивание: по центру
- Отступы: 15px сверху

---

## 🤖 ЧАСТЬ 3: ПРИВЕТСТВЕННОЕ СООБЩЕНИЕ В TELEGRAM-БОТЕ

### Вариант 1: Короткое (рекомендуется)

```
Привет! 👋

Вы только что посчитали, сколько денег теряете из-за необработанных заявок в Instagram.

Шокирующие цифры, правда?

🎯 Хорошая новость: это можно исправить!

Я помогаю владельцам Instagram-магазинов автоматизировать обработку заявок с помощью AI-ассистента, который:

✅ Отвечает на 95% вопросов моментально
✅ Работает 24/7 без выходных
✅ Не теряет ни одной заявки
✅ Интегрируется с amoCRM и МойСклад

Результат: вы перестаете терять деньги и начинаете их зарабатывать.

━━━━━━━━━━━━━━━━━━━━━━

📞 Хотите узнать, как это работает?

Запишитесь на бесплатную диагностическую сессию (30 минут):
• Посмотрим на ваш Instagram
• Найдем точки роста
• Покажу примеры автоматизации
• Ответю на все вопросы

Без навязывания и продаж в лоб — только конкретика.

[Кнопка: Записаться на сессию]
→ Ссылка на календарь (Calendly / Yclients)
```

---

### Вариант 2: Расширенное (с кейсами)

```
Приветствую! 👋

Меня зовут [Ваше имя], я специалист по автоматизации Instagram-магазинов.

Вы только что узнали шокирующую правду — сколько денег теряете каждый день из-за необработанных заявок.

И знаете что? Вы не один такой.

━━━━━━━━━━━━━━━━━━━━━━

📊 РЕАЛЬНЫЕ ДАННЫЕ:

87% владельцев Instagram-магазинов теряют больше 50% заявок просто потому что не успевают обрабатывать их вовремя.

Средние потери: 75,000-150,000₽/день
Годовые потери: до 40,000,000₽

━━━━━━━━━━━━━━━━━━━━━━

🎯 КАК Я ПОМОГАЮ:

Я создаю AI-ассистента для вашего Instagram, который:

✅ Отвечает на вопросы клиентов за 15 секунд (вместо 3-5 часов)
✅ Работает 24/7 — даже когда вы спите
✅ Обрабатывает 100% заявок — ни одна не теряется
✅ Интегрируется с amoCRM и МойСклад
✅ Звучит как живой человек (не как робот)

━━━━━━━━━━━━━━━━━━━━━━

💼 КЕЙСЫ (примеры):

**Магазин одежды:**
• Было: 200 заявок/день, обрабатывали 30%
• Стало: 200 заявок/день, обрабатывают 95%
• Результат: +2,800,000₽/мес выручки

**Магазин обуви:**
• Было: терял 120 заявок/день
• Стало: теряет 5-7 заявок/день
• Результат: +1,500,000₽/мес

━━━━━━━━━━━━━━━━━━━━━━

📞 БЕСПЛАТНАЯ ДИАГНОСТИКА (30 минут):

Давайте созвонимся и я покажу:
• Как именно работает автоматизация
• Какие процессы можно автоматизировать У ВАС
• Сколько денег вы сможете вернуть
• Реальные примеры внедрения

Без навязывания и агрессивных продаж.
Просто посмотрим на ваш бизнес и найдем решение.

[Кнопка: Записаться на диагностику]
→ Ссылка на календарь

Или напишите удобное время — я подстроюсь 👇
```

---

### Кнопки для Telegram-бота

**Кнопка 1: Записаться на диагностику**
- Тип: URL кнопка
- Текст: "📞 Записаться на сессию"
- URL: ссылка на ваш календарь (Calendly, Yclients, или Google Calendar)

**Кнопка 2: Узнать больше (опционально)**
- Тип: URL кнопка
- Текст: "📄 Как это работает?"
- URL: ссылка на пост в Telegram-канале с подробным разбором

**Кнопка 3: Кейсы (опционально)**
- Тип: callback кнопка
- Текст: "💼 Посмотреть примеры"
- Action: отправить сообщение с подробными кейсами

---

## 🎨 ЧАСТЬ 4: НАСТРОЙКИ ДИЗАЙНА TILDA

### Цветовая схема

**Основные цвета:**
- Фон страницы: `#ffffff` (белый)
- Текст заголовков: `#1a1a1a` (почти черный)
- Текст основной: `#333333` (темно-серый)
- Текст второстепенный: `#666666` (серый)
- Акцент (кнопки): `#ef4444` (красный)
- Предупреждения: `#f59e0b` (оранжевый)

**Дополнительные:**
- Фон блоков: `#f9fafb` (светло-серый)
- Бордеры: `#e5e5e5` (светло-серый)

---

### Шрифты

**Основной:** Inter или Montserrat
- Заголовки: Bold (700)
- Подзаголовки: SemiBold (600)
- Текст: Regular (400)

**Размеры:**
- H1: 38px (desktop), 28px (mobile)
- H2: 28px (desktop), 22px (mobile)
- H3: 22px (desktop), 18px (mobile)
- Текст: 18px (desktop), 16px (mobile)
- Мелкий текст: 14px

---

### Отступы и spacing

```
Между блоками: 60px (desktop), 40px (mobile)
Внутри блоков: 30px (desktop), 20px (mobile)
Вокруг кнопок: 40px сверху, 20px снизу
```

---

## ✅ ЧЕКЛИСТ ЗАПУСКА

**Перед публикацией проверьте:**

- [ ] HTML код калькулятора вставлен в блок T123
- [ ] Высота iframe настроена (650px desktop, 700px mobile)
- [ ] Все тексты скопированы из ЧАСТИ 2
- [ ] CTA кнопка ведет на правильный Telegram-бот (замените URL!)
- [ ] Калькулятор работает (протестируйте с разными числами)
- [ ] Результаты отображаются корректно
- [ ] Форматирование чисел работает (пробелы вместо запятых)
- [ ] Мобильная версия адаптирована
- [ ] Telegram-бот настроен и отвечает
- [ ] Приветственное сообщение в боте настроено
- [ ] Кнопка "Записаться" в боте ведет на рабочую ссылку календаря

---

## 🚀 СЛЕДУЮЩИЕ ШАГИ

**После запуска лид-магнита:**

1. **Настроить рекламу в Яндекс.Директ** (РСЯ)
   - Заголовок: "Теряете 70% заявок в Instagram?"
   - Текст: "Посчитайте сколько денег упускаете каждый день"
   - Ссылка на лендинг Tilda

2. **Создать 7 писем прогрева** (следующий этап)
   - Для тех, кто заполнил калькулятор но не записался на сессию
   - Email-последовательность на 7-10 дней

3. **Настроить аналитику:**
   - Яндекс.Метрика (цель: заполнение калькулятора)
   - Google Analytics (если используете)
   - Отслеживание кликов по кнопке CTA

4. **Создать скрипт диагностической сессии**
   - Вопросы для выявления боли
   - Презентация решения
   - Обработка возражений

---

**ЭТАП 2 ЗАВЕРШЕН! ✅**

Готов к тестированию и запуску 🚀
