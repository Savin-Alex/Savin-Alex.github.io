# Alexander Savin

[RU](https://savin-alex.github.io/) · [EN](https://savin-alex.github.io/en/)

## IT Project Manager

Веду сложные AI-проекты от идеи до релиза. Отвечаю за весь путь: что делаем, что считаем готовым, как проверяем и что может пойти не так.

До IT — пять лет руководства инженерными проектами: бюджеты до 130 млн ₽+, команды до 15 человек и 5+ подрядчиков.

[Сайт портфолио](https://savin-alex.github.io/)

---

## Что я приношу в проект

- Опыт управления: пять лет руководства проектами с бюджетами до 130 млн ₽+ и командами до 15 человек.
- Девять кейсов в портфолио, семь из них — AI-продукты. Два работают в продакшене: [DVPhoto.me](https://dvphoto.me) и Telegram-канал [«ИИнтересные новости»](https://t.me/ainews24by7).
- Проектные артефакты, по которым видно состояние проекта: устав, roadmap, бэклог, критерии приёмки, Definition of Done, реестр рисков и журнал решений.
- Техническая база: Python, API, Jira, GitHub, CI. Могу разобрать оценку разработчика и заметить риск до того, как он сорвёт срок.

---

## Ключевые проекты

### Multi-Agent AI Development Pipeline

Прототип управляемого AI-assisted delivery-процесса: от задачи в Jira до проверяемого GitHub PR с участием человека, критериями качества и evidence package.

- Сформировал проектный контур: charter, roadmap, backlog, acceptance criteria, Definition of Done, risk register и decision log.
- Спроектировал workflow Jira → planning → implementation → validation → review → PR с dry-run и controlled real-run режимами.
- Определил AI governance, quality gates и метрики процесса: success rate, retry rate, cost per run, stage cycle time и lead time to PR.

Стек: Python, Claude API, Jira Cloud, GitHub API, GitHub Actions, pytest

[Открыть страницу проекта](projects/ai-dev-pipeline.md)

---

### AI Consul

Privacy-first AI-ассистент для live-разговоров, интервью, встреч и образовательных сессий.

- Спроектировал desktop-продукт с live transcription, companion overlay и session control.
- Определил local-first/hybrid подход к обработке аудио с явными privacy и fallback controls.
- Вынес engine status, cost awareness и privacy settings в интерфейс, чтобы технические риски были видимы пользователю.

Стек: Electron, TypeScript, React, Python/WhisperLiveKit, Ollama

[Открыть case study](projects/ai-consul.md)

---

### Local PII Anonymizer

Локальный offline-сервис обратимой псевдонимизации ПДн и PHI для RU/EN документов под compliance-контуры: 152-ФЗ, GDPR, HIPAA Safe Harbor.

- Каскадная детекция: regex с checksum → Presidio/spaCy NER → опциональный Medical Transformer → LLM-fallback.
- 17+ типов сущностей с валидацией контрольных сумм; обратимая псевдонимизация через pluggable Vault.
- Eval-driven CI: macro F1 ≈ 0.9997 (15/17 типов = 1.0) на synthetic-корпусе; 367 тестов, 15 ADR.
- Август 2026: движок стал ядром privacy-шлюза с версионированным API, принятым после независимого ревью; 34 ADR.

Стек: Python 3.12, FastAPI, Streamlit, Pydantic, Presidio, spaCy, Docker

[Открыть страницу проекта](projects/local-pii-anonymizer.md)

---

### RemovePII

Веб-приложение для частных пользователей: защищает текст и документы перед отправкой в AI-инструменты и восстанавливает ответ. Исходный текст не хранится.

- Защита DOCX, текстовых PDF, CSV и JSON с сохранением формата; экран проверки найденного перед отправкой.
- 18 типов учётных данных (API-ключи, приватные ключи, строки подключения); изоляция данных по аккаунтам.
- 1252 Python-теста и 140 веб-тестов; аудит репозитория: 18 находок, первый проход исправлений завершён.

Стек: Python, FastAPI, PostgreSQL, Redis, Presidio, Next.js

[Открыть страницу проекта](projects/removepii.md)

---

### DVPhoto.me

Trust-first сервис проверки фотографий для DV Lottery: 32 проверки, 11 языков интерфейса и детерминированный auto-fix без дорисовки и generative edits. Работает на VPS.

- Провёл аудит полного цикла на 763 фото и 1152 внесённых дефектах: 0 падений, 1,0% дефектов прошли незамеченными.
- Аудит показал, что дорисовка полей давала брак (4 из 10 выданных файлов); исправление теперь только кадрирует, уменьшает и пересжимает.
- Проверки с низкой precision (Face Lighting — 6,4%) оставлены предупреждениями; выходной шлюз блокирует любой критический отказ.

[Открыть страницу проекта](projects/dvphoto.md)

---

### PII Firewall

Браузерное расширение, которое маскирует ПДн до отправки промпта в AI-чат и восстанавливает их в ответе локально. Ноль сетевых разрешений.

- 36 типов данных (РФ, США, Великобритания, Китай, ОАЭ, MRZ, секреты, ФИО с падежами) с проверкой контрольных сумм.
- Guard-режим перехватывает отправку; опциональное хранилище AES-GCM.
- 117 тестов и синтетический корпус из 123 случаев; пакет v0.5.0 готов к Chrome Web Store.

Стек: TypeScript, Manifest V3, Vite, Web Crypto, Vitest

[Открыть страницу проекта](projects/pii-firewall.md)

---

### Telegram AI News

Поэтапный LLM-пайплайн для Telegram-канала про ИИ: extract → score → synthesize. Канал работает автоматически; сами публикуются только источники высшего доверия.

- Код может только ужесточить вердикт модели; граница затрат по уровню доверия источника и breaker по оплате.
- Учёт модели, этапа, токенов и стоимости каждого LLM-вызова.
- 243 теста, CI с pip-audit; 13 находок security-аудита устранены.

Стек: Python, FastAPI, SQLite, Anthropic SDK, Ollama, python-telegram-bot

Канал: [ИИнтересные новости](https://t.me/ainews24by7)

[Открыть страницу проекта](projects/telegram-ai-news.md)

---

### Mood&Food

PM/BA-кейс мобильного приложения для сети ресторанов, который показывает полный цикл планирования и защиты решения.

- Сформировал project charter, требования, MVP scope, roadmap, оценку, график и Jira backlog.
- Приоритизировал 40+ функций по MoSCoW и собрал реестр из 50+ рисков.
- Подготовил Figma-прототип и stakeholder presentation для защиты решения.

[Открыть страницу проекта](projects/mood-food.md)

---

### DemoTel KZ

Детерминированный Telegram-бот поддержки для телекома: 50 сценариев, PII-redaction и отдельные безопасные маршруты для fraud/SIM-swap.

- Осознанно отказался от LLM на критическом пути ради предсказуемости и полного тестирования.
- 84/84 теста; высокорисковые сценарии всегда обходят L1 automation.
- Production-интеграция с billing/CRM обозначена как out of scope, а не как готовый результат.

[Открыть страницу проекта](projects/telecombot.md)
