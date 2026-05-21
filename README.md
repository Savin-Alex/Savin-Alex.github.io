# Alexander Savin

[RU](https://savin-alex.github.io/) · [EN](https://savin-alex.github.io/en/)

## AI Project Manager / Technical Project Manager

Управляю AI- и IT-инициативами от идеи до пилота и готового к внедрению delivery-контура: бэклог, критерии приёмки, quality gates, управление рисками, коммуникации со стейкхолдерами и измеримые результаты.

Фокус: GenAI/LLM workflow, AI governance, продуктовые решения с учётом privacy, автоматизация delivery и technical project management.

[Сайт портфолио](https://savin-alex.github.io/)

---

## Что я приношу в проект

- 5 лет в инженерных и проектных ролях; последние проекты сфокусированы на AI-delivery, GenAI/LLM workflow и privacy-aware продуктах.
- Практический опыт AI/LLM-проектов: агентные workflow, продукты с транскрибацией, privacy-first обработка данных, eval-driven delivery.
- PM-артефакты, которые делают delivery управляемым и проверяемым: project charter, roadmap, backlog, acceptance criteria, Definition of Done, risk register, decision log, evidence package и stakeholder demo.
- Техническую грамотность для AI-проектов: Python, API, Jira, GitHub, CI, основы system design, prompt engineering и quality gates.

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

Стек: Electron, TypeScript, React, Python, whisper.cpp

[Открыть case study](projects/ai-consul.md)

---

### Local PII Anonymizer

Локальный offline-сервис обратимой псевдонимизации ПДн и PHI для RU/EN документов под compliance-контуры: 152-ФЗ, GDPR, HIPAA Safe Harbor.

- Каскадная детекция: regex с checksum → Presidio/spaCy NER → опциональный Medical Transformer → LLM-fallback.
- 17+ типов сущностей с валидацией контрольных сумм; обратимая псевдонимизация через pluggable Vault.
- Eval-driven CI: macro F1 ≈ 0.9997 (15/17 типов = 1.0) на synthetic-корпусе; 367 тестов, 15 ADR.

Стек: Python 3.12, FastAPI, Streamlit, Pydantic, Presidio, spaCy, Docker

[Открыть страницу проекта](projects/local-pii-anonymizer.md)

---

### Mood&Food

PM/BA-кейс мобильного приложения для сети ресторанов.

- Провёл полный project-management цикл: паспорт проекта, коммерческое предложение, требования, MVP scope, roadmap, оценка, план-график, Jira backlog и stakeholder presentation.
- Приоритизировал 40+ функций по MoSCoW и сформировал MVP первого релиза.
- Собрал Figma-прототипы, чтобы сделать требования наглядными и ускорить обсуждение пользовательских сценариев.

Стек: Figma, GanttPRO, Jira

[Открыть страницу проекта](projects/mood-food.md)
