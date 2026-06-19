---
title: Local PII Anonymizer
---

# Local PII Anonymizer

[RU](./local-pii-anonymizer.md) · [EN](../en/projects/local-pii-anonymizer.md)

Локальный (офлайн) сервис обратимой псевдонимизации ПДн и PHI для документов на русском и английском. Сделан под требования 152-ФЗ, GDPR и HIPAA Safe Harbor — чтобы юристы, HR и клиники могли безопасно готовить конфиденциальные документы для внешних LLM, сохраняя обратимые соответствия под контролем оператора.

<figure class="demo-figure">
<iframe class="demo-frame demo-pii" src="{{ '/demos/local-pii-anonymizer.ru.html' | relative_url }}" title="Интерактивное демо: Local PII Anonymizer" loading="lazy"></iframe>
<figcaption style="font-size:.9em;color:#667085;margin-top:.4em;">Анимация: затирание данных, поток через каскад детекции и зашифрованный vault. <a href="{{ '/demos/local-pii-anonymizer.ru.html' | relative_url }}" target="_blank" rel="noopener">Открыть в полном окне ↗</a></figcaption>
</figure>

## Паспорт проекта

| Поле | Содержание |
|---|---|
| **Проблема** | Готовые решения по анонимизации либо облачные (сама по себе утечка), либо однопроходные regex-фильтры, которые ломаются на смешанных русско-английских документах и не проверяют контрольные суммы. Для регулируемых отраслей этого мало. |
| **Цель** | Local-first инструмент псевдонимизации PII/PHI: безопасно готовить чувствительные документы (RU/EN) для LLM, сохраняя обратимые соответствия под контролем оператора. |
| **В scope** | Каскадная детекция (regex+checksum → Presidio/spaCy → Medical NER → LLM-fallback); 17+ типов сущностей; reversible pseudonymization + HIPAA Safe Harbor обезличивание; зашифрованный vault (AES-256-GCM) + tenant-изоляция; FastAPI/Streamlit/CLI/Docker; eval-харнесс + CI-гейт; audit trail. |
| **Вне scope** | Сертифицированный production-compliance; валидация на реальных клиентских документах; полный retention/deletion governance; production-деплой (TLS / secret store / append-only audit); Windows-валидация. |

**Статус: Amber** — сильный технический прогресс; пока не одобрено для production-PII без финального security / privacy / legal review и deployment-контролей.

**Критерии успеха (со статусом):**
- Чёткая терминология (псевдонимизация ≠ анонимизация) → **выполнено**
- Зашифрованный, tenant-изолированный vault → **выполнено** (AES-256-GCM, tenant-scoped restore/delete)
- Измеренное качество детектора → **выполнено на синтетике** (macro F1 ≈ 0.9997; 15/17 типов = 1.0) — не на реальных документах
- Аудируемость операций → **выполнено** (audit events, метрики)
- Путь от пилота до compliant production → **задокументирован, не пройден**

## Моя роль

**Роль:** Junior IT PM · соло-проект (цель — production); на мне постановка задачи, архитектура, разработка и контроль качества.

- Сформулировал постановку задачи для регулируемых сценариев: юристы, HR, медицина.
- Определил режимы с учётом compliance: обратимая псевдонимизация и обезличивание по HIPAA Safe Harbor.
- Спроектировал архитектуру с явными quality gates: tests, eval harness, CI hard-fail, audit trail, PII-safe logging.
- Собрал технический MVP, который подаётся как кейс по AI governance и управлению риском данных, а не только как backend-сервис.

## Решение

Каскадная архитектура детекции с явными compliance-режимами и обратимым vault-хранилищем псевдонимов.

- Каскад: regex с checksum → Presidio (spaCy NER) → опциональный Medical NER (HuggingFace) → опциональный LLM-fallback (Ollama, через circuit breaker).
- 17+ типов сущностей: ФИО, паспорт РФ (3 формата), ИНН/СНИЛС/ОГРН/БИК/КПП с валидацией, кадастровый номер, VIN, госномер, US SSN, UK NINO, MAC, SWIFT/BIC, AWS keys, JWT и т.д.
- Обратимая псевдонимизация через сменный Vault: InMemory / SQLite / Redis; sharded lock pool для конкурентного доступа без утечек памяти.
- HIPAA Safe Harbor mode — необратимое обезличивание дат (year-only) и возрастов больше 89 (`90+`), без записи в vault.
- Audit trail в structlog: 152-ФЗ / GDPR Art. 30 / HIPAA §164.312.

## Архитектура

Clean Architecture с чёткими границами слоёв и port-adapter pattern.

- `domain/` — сущности, value objects, политики режимов.
- `application/` — use-cases, оркестрация каскада детекции, decorator-цепочка (FilteringEngine).
- `infrastructure/` — Presidio/spaCy/HF-адаптеры, Vault-репозитории (Protocol-ports + ABC), HTTP/CLI/UI слой.
- `tests/` — unit, integration, property-based (Hypothesis), eval-harness.

UI-поверхности: FastAPI HTTP, Streamlit-демо, CLI batch-режим, Docker Compose.

## Метрики (snapshot)

| Область | Сигнал |
|---|---|
| Тестов собрано | 367 (43 тест-файла) |
| ADR | 15 |
| Eval-корпус | 4 curated golden + 200 синтетических документов |
| Качество eval | macro F1 ≈ 0.9997 (presidio_multi); 15/17 типов на F1 = 1.0 |
| Точки входа | FastAPI, Streamlit UI, CLI, Docker Compose |
| Vault-бэкенды | Memory, SQLite, Redis |
| Форматы документов | txt, md, csv, json, pdf, docx |
| CI-проверки | mypy strict, ruff, bandit, pip-audit, eval hard-fail gate |

Числа — на синтетическом корпусе, как внутренний eval gate, а не production-гарантия.

## Risk Register

| Риск | Уровень | Владелец | Ответ |
|---|---|---|---|
| Compliance-scope формально не утверждён (GDPR/HIPAA/152-ФЗ…) | Выс. | Legal / Compliance | Подтвердить применимые режимы и собрать review-пакет до запуска на реальных ПДн. |
| Recall детектора не доказан на реальных документах | Выс. | Product / ML | Добавить анонимизированные «клиентоподобные» golden-данные, задать launch-гейты. |
| Retention / deletion / backup / key-rotation не завершены | Выс. | Security / Platform | Определить retention-политику, deletion SLA, backup erasure, миграцию ротации ключей. |
| Production-контроли деплоя не финализированы | Сред. | Platform | Залочить зависимости, security-сканы, профиль TLS / secret / audit / metrics. |
| Windows-поддержка не валидирована | Сред. | Engineering | Добавить Windows CI и smoke-тест на Windows 11. |

## Надёжность и безопасность

- Зашифрованный vault at rest (AES-256-GCM), AAD-binding строк, fail-closed поведение шифрования в CLI.
- Tenant-изоляция: scoped restore/delete, tenant-based rate limit, tenant в audit trail.
- Non-root / read-only контейнеры, секреты через compose secret files.
- Lazy model loading + FastAPI lifespan (нет холодного старта на первом запросе); circuit breaker для LLM-fallback.
- structlog с PII-redactor — даже логи не должны утекать.

## Статус и next steps

Полный one-pager: [Status report (sample)](/evidence/pii-status-report.html).

**Сделано:** security hardening (зашифрованный vault, AES-GCM AAD, fail-closed CLI, tenant-scoped restore/delete, non-root контейнеры); терминология исправлена (псевдонимизация, не анонимизация; явные Safe Harbor caveats); evidence-база (15 ADR, C4/data-flow, regulatory mapping, runbook, eval summary).

**Решения нужны:** scope продукта (пилот / beta / production?); compliance-режимы в launch-scope (GDPR / CCPA / HIPAA / 152-ФЗ / SOC 2?); цель деплоя; data-policy (retention, deletion SLA, key custody); стандарт оценки (кто владеет реальными golden-данными, какие F1/recall-гейты приемлемы).

**Next steps:** доделать extractor (no-text PDF) + полный прогон unit/eval/security; Windows CI + smoke-тест; production deployment profile (TLS, secret store, encrypted storage, audit sink, dependency lock); privacy review packet (data inventory, DPIA, retention/deletion policy); pilot go/no-go review (риски, evidence, rollback).

## Ретро

- **Сработало:** безопасность и приватность перешли из прототипа в архитектуру, готовую к ревью.
- **Улучшить:** доказательную базу нужно сместить с синтетической уверенности на проверку на реальных «клиентоподобных» документах.
- **Продолжать:** строгая терминология; явные риски и решения, подкреплённые audit-trail, — до расширения scope.

## Стек

Python 3.12, FastAPI, Streamlit, Pydantic, structlog, Docker, Presidio, spaCy, HuggingFace Transformers, Ollama, Hypothesis, pytest, mypy, ruff, bandit.
