# Карточки проектов

Готовые формулировки для главной страницы, GitHub README или дизайн-макета.

## Multi-Agent AI Development Pipeline

### Короткая версия
**Multi-Agent AI Development Pipeline**
Прототип управляемого AI-assisted delivery-процесса: от задачи в Jira до проверяемого GitHub PR с участием человека, критериями качества и evidence package.

### Текст карточки
- Сформирован проектный контур: charter, roadmap, backlog, acceptance criteria, Definition of Done, risk register и decision log.
- Спроектирован workflow Jira → planning → implementation → validation → review → PR с dry-run и controlled real-run режимами.
- Определены AI governance, quality gates и метрики процесса: success rate, retry rate, cost per run, stage cycle time и lead time to PR.

**Стек:** Python, Claude API, Jira Cloud, GitHub API, GitHub Actions, pytest
**CTA:** `Открыть страницу проекта`

## AI Consul

### Короткая версия
**AI Consul**
Privacy-first AI-ассистент для live-разговоров, интервью, встреч и образовательных сессий.

### Текст карточки
- Спроектирован desktop-продукт с live transcription, companion overlay и session control.
- Определён local-first/hybrid подход к обработке аудио с явными privacy и fallback controls.
- Engine status, cost awareness и privacy settings вынесены в интерфейс, чтобы технические риски были видимы пользователю.

**Стек:** Electron, TypeScript, React, Python/WhisperLiveKit, Ollama
**CTA:** `Открыть case study`

## Local PII Anonymizer

### Короткая версия
**Local PII Anonymizer**
Локальный offline-сервис обратимой псевдонимизации PII/PHI для RU/EN документов под 152-ФЗ, GDPR и HIPAA Safe Harbor.

### Текст карточки
- Каскадная детекция: regex с checksum → Presidio/spaCy NER → optional Medical Transformer → LLM fallback.
- 17+ типов сущностей с checksum validation и reversible pseudonymization через pluggable Vault.
- Eval-driven CI gate: macro F1 ≈ 0.9997 (15/17 entity types = 1.0) on a synthetic corpus, 367 tests, 15 ADR.

**Стек:** Python 3.12, FastAPI, Streamlit, Pydantic, Presidio, spaCy, Docker
**CTA:** `Открыть страницу проекта`

## DVPhoto.me

### Короткая версия
**DVPhoto.me**
Trust-first сервис проверки DV-фотографий: 32 проверки, аудит на 763 фото и auto-fix, который только кадрирует, уменьшает и пересжимает.

### Текст карточки
- Аудит полного цикла: 763 фото, 1152 внесённых дефекта, 0 падений, 1,0% дефектов прошли незамеченными.
- Аудит нашёл брак от дорисовки полей (4 из 10 файлов) — дорисовка и увеличение убраны из продукта.
- Проверки с низкой precision остаются предупреждениями; выходной шлюз блокирует любой критический отказ.

**Стек:** Python, FastAPI, MediaPipe Tasks, OpenCV, PostgreSQL, Docker, Coolify
**CTA:** `Открыть case study`

## Mood&Food

### Короткая версия
**Mood&Food**
PM/BA-кейс мобильного приложения для сети ресторанов: требования, MVP, roadmap, оценка, прототип и stakeholder presentation.

### Текст карточки
- Полный project-management цикл от идеи до защиты решения.
- 40+ функций приоритизированы по MoSCoW; сформирован MVP первого релиза.
- Figma prototype, GanttPRO schedule, estimate и Jira backlog для команды из 4 человек.

**Стек:** Figma, GanttPRO, Jira, MoSCoW
**CTA:** `Открыть case study`

## DemoTel KZ

### Короткая версия
**DemoTel KZ**
Детерминированный Telegram-бот поддержки: 50 сценариев, PII-redaction и safety-first routing без LLM.

### Текст карточки
- Высокорисковые сценарии всегда обходят L1 automation и уходят во Fraud / L2.
- ПДн маскируются до маршрутизации и логирования.
- 84/84 теста; production-интеграция с оператором честно обозначена как out of scope.

**Стек:** Python, FastAPI, PostgreSQL, Telegram, Docker
**CTA:** `Открыть case study`

## RemovePII

### Короткая версия
**RemovePII**
Персональный privacy-шлюз: защищает текст и документы перед отправкой в AI-инструменты, исходный текст не хранится.

### Текст карточки
- DOCX, текстовые PDF, CSV и JSON возвращаются в том же формате с замаскированными значениями.
- Экран проверки перед отправкой; маппинги зашифрованы и быстро истекают.
- 1392 теста; 18 типов учётных данных; аудит репозитория с исправлениями через регрессионные тесты.

**Стек:** Python, FastAPI, PostgreSQL, Redis, Presidio, Next.js
**CTA:** `Открыть страницу проекта`

## PII Firewall

### Короткая версия
**PII Firewall**
Браузерное расширение без сетевых разрешений: маскирует ПДн до отправки в AI-чат и восстанавливает их в ответе локально.

### Текст карточки
- 36 типов данных с проверкой контрольных сумм и уровнями уверенности.
- Guard-режим перехватывает отправку; privacy-заявление проверяется в manifest.
- 117 тестов, синтетический корпус из 123 случаев; v0.5.0 готов к Chrome Web Store.

**Стек:** TypeScript, Manifest V3, Vite, Web Crypto, Vitest
**CTA:** `Открыть страницу проекта`

## Telegram AI News

### Короткая версия
**Telegram AI News**
Система читает новости про ИИ, отбирает важные и пишет короткие посты. Канал работает автоматически; сами публикуются только источники высшего доверия. Канал: https://t.me/ainews24by7

### Текст карточки
- Extract → score → synthesize со structured output; код может только ужесточить вердикт модели.
- Граница затрат, учёт стоимости каждого вызова и breaker по оплате.
- 243 теста, CI с pip-audit, 13 находок security-аудита устранены.

**Стек:** Python, FastAPI, SQLite, Anthropic SDK, Ollama, python-telegram-bot
**CTA:** `Открыть страницу проекта`

## Однострочное описание портфолио

> Девять кейсов, которые показывают technical AI delivery: governance, realtime, computer vision, privacy/compliance, контроль LLM-затрат, safety и проверяемую поставку.
