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

**Стек:** Electron, TypeScript, React, Python, whisper.cpp
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

## Mood&Food

### Короткая версия
**Mood&Food**  
PM/BA-кейс мобильного приложения для сети ресторанов: требования, MVP, roadmap, оценка, прототип и stakeholder presentation.

### Текст карточки
- Полный project-management цикл от идеи до защиты решения.
- 40+ функций приоритизированы по MoSCoW; сформирован MVP первого релиза.
- Figma prototype, GanttPRO schedule, estimate и Jira backlog для команды из 4 человек.

**Стек:** Figma, GanttPRO, Jira
**CTA:** `Открыть прототип`

## Однострочное описание портфолио

> Четыре проекта, которые показывают мой подход к AI delivery, privacy/compliance, PM/BA-артефактам и управляемой поставке IT-решений.
