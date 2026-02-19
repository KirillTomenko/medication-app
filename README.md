# 💊 Medication Tracker

> Простое PWA-приложение для ежедневного отслеживания приёма лекарств с синхронизацией в Google Sheets.

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![PWA](https://img.shields.io/badge/PWA-5A0FC8?style=flat&logo=pwa&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)

---

## 🎯 Возможности

- 📅 **Интерактивный календарь** — цветовая индикация дней: 🟢 все принято / 🟠 частично / серый — не отмечено
- ✅ **Отметка приёма** — одно нажатие фиксирует факт приёма препарата
- 📝 **Заметки** — до 50 символов комментария к каждому дню
- 🔔 **Push-уведомления** — напоминания в **07:30** и **21:00** через Service Worker
- ☁️ **Синхронизация с Google Sheets** — данные уходят в таблицу автоматически после отметки
- 📱 **PWA** — устанавливается на главный экран как нативное приложение
- 💾 **Офлайн-хранение** — все данные сохраняются в `localStorage`, работает без интернета

---

## 🚀 Быстрый старт

1. Скачайте или клонируйте репозиторий:
   ```bash
   git clone https://github.com/kiboto30-creator/medication-tracker.git
   ```
2. Откройте `index.html` в браузере — приложение готово к работе без сборки и зависимостей.
3. *(Опционально)* Настройте синхронизацию с Google Sheets (см. ниже).

### Установка как PWA (мобильный)

- **Android (Chrome):** меню → «Добавить на главный экран»
- **iOS (Safari):** кнопка «Поделиться» → «На экран "Домой"»

---

## ⚙️ Настройка Google Sheets

### Шаг 1 — Создайте скрипт

1. Откройте [Google Sheets](https://sheets.google.com) и создайте новую таблицу.
2. Перейдите: **Расширения → Apps Script**.
3. Удалите весь код по умолчанию и вставьте:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSheet();
    var data  = JSON.parse(e.postData.contents);
    sheet.appendRow([
      data.date,
      data.med1    ? '✅' : '❌',
      data.med2    ? '✅' : '❌',
      data.comment || ''
    ]);
    return ContentService
      .createTextOutput(JSON.stringify({ status: 'success' }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch(err) {
    return ContentService
      .createTextOutput(JSON.stringify({ status: 'error', message: err.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

// Обработчик preflight-запросов
function doGet(e) {
  return ContentService
    .createTextOutput(JSON.stringify({ status: 'ok' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

### Шаг 2 — Разверните как веб-приложение

| Параметр | Значение |
|---|---|
| Тип | **Веб-приложение** |
| Выполнять как | **Я** (ваш Google-аккаунт) |
| Кто имеет доступ | **Все** |

> ⚠️ Нажмите **«Новое развертывание»**, а не «Обновить существующее». После каждого изменения кода — снова «Новое развертывание» и копируйте новый URL.

### Шаг 3 — Вставьте URL в приложение

Скопируйте URL вида `https://script.google.com/macros/s/AKfy.../exec` и нажмите **✏️ Изменить URL** в приложении.

---

## 📋 Структура данных в таблице

| Столбец A | Столбец B | Столбец C | Столбец D |
|---|---|---|---|
| Дата (YYYY-MM-DD) | Препарат A | Препарат B | Заметка |
| 2025-06-15 | ✅ | ❌ | Голова болела |

---

## 🐛 Решение проблем

**Данные не попадают в таблицу:**
- Убедитесь, что URL заканчивается на `/exec`, а не `/dev`
- Проверьте, что при развёртывании выбрано **«Все»** в поле доступа
- После изменений в скрипте создайте **новое** развёртывание и обновите URL в приложении
- Откройте консоль браузера (F12 → Console) и проверьте наличие ошибок

**Уведомления не приходят:**
- Разрешите уведомления в настройках браузера для этой страницы
- Страница должна быть открыта в браузере (Service Worker не может разбудить браузер)
- Проверьте, что переключатель уведомлений в приложении включён

**Данные исчезли:**
- Данные хранятся в `localStorage` — они привязаны к браузеру и домену
- Очистка данных браузера удаляет историю приёма

---

## 🛠️ Технологии

- **HTML5 / CSS3 / Vanilla JS** — без фреймворков и зависимостей
- **localStorage** — хранение данных на устройстве
- **Service Worker** — push-уведомления и PWA-поддержка
- **Web Notifications API** — нативные уведомления
- **Google Apps Script** — серверная часть для Google Sheets

---

## 📁 Структура проекта

```
medication-tracker/
└── index.html    # Всё приложение в одном файле
```

---

## 📱 Совместимость

| Браузер | Поддержка |
|---|---|
| Chrome / Edge | ✅ Полная (рекомендуется) |
| Firefox | ✅ Полная |
| Safari (iOS / macOS) | ✅ PWA и уведомления |
| Мобильные браузеры | ✅ |

---

## 📄 Лицензия

[MIT](LICENSE)

---

## 👤 Автор

**kiboto30-creator** — [GitHub](https://github.com/kiboto30-creator)

Приветствуются [Issues](https://github.com/kiboto30-creator/medication-tracker/issues) и Pull Requests с предложениями!
