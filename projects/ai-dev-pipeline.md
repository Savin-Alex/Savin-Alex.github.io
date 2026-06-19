---
title: AI Delivery Pipeline
---

# Multi-Agent AI Development Pipeline

[RU](./ai-dev-pipeline.md) · [EN](../en/projects/ai-dev-pipeline.md)

Production-like прототип (2026): процесс из трёх AI-агентов, который проводит небольшую задачу от Jira до готового Pull Request — с участием человека, проверками качества и прослеживаемостью решений.

Идея — показать, что GenAI/LLM в разработке — это не «автогенерация кода», а управляемый процесс с контролем качества, рисков и стоимости.

<figure class="demo-figure">
<iframe class="demo-frame demo-pipeline" src="{{ '/demos/ai-dev-pipeline.ru.html' | relative_url }}" title="Интерактивное демо: Multi-Agent AI Development Pipeline" loading="lazy"></iframe>
<figcaption style="font-size:.9em;color:#667085;margin-top:.4em;">Интерактивная анимация процесса Jira → PR. <a href="{{ '/demos/ai-dev-pipeline.ru.html' | relative_url }}" target="_blank" rel="noopener">Открыть в полном окне ↗</a></figcaption>
</figure>

## Паспорт проекта

| Поле | Содержание |
|---|---|
| **Проблема** | Демо AI-кодинга — одношаговый сценарий (вставил задачу → получил код → проверил вручную). Продуктивности это помогает, но это не процесс поставки: нет приёма задачи, декомпозиции, проверки, ревью, прослеживаемости и ответственности человека. |
| **Цель** | Показать, как GenAI встраивается в поставку не как «автогенерация кода», а как контролируемый workflow с acceptance criteria, quality gates, управлением рисками, метриками и evidence-пакетом. |
| **В scope** | Приём Jira-задачи → планирование → реализация → проверка в sandbox → ревью → GitHub PR; артефакты прогона; план отката; production-like governance через GitHub Actions. |
| **Вне scope** | Параллельное исполнение; дополнительные агенты (DevOps / документация); production-контейнеризация; автоматизация больших расплывчатых задач. |

**Критерии успеха (со статусом):**
- Результат AI не считается доверенным до явных проверок (тесты, sandbox, secret scan, ревью) → **выполнено**
- dry-run по умолчанию, real-run только под контролем (GitHub Actions) → **выполнено**
- Локальный контур стабилен → **выполнено** (55 тестов, lint чистый)
- Артефакты по каждому прогону (summary, метрики, логи) → **выполнено** (150 run summaries по 11 issue-группам)
- Production-деплой, dashboard метрик, approval matrix → **не выполнено** (дальнейшие шаги)

## Моя роль

**Роль:** Junior IT Project Manager · соло-проект (прототип).

- Отвечал за постановку цели, scope, backlog, roadmap, Definition of Done, risk register, quality gates, метрики и документацию для проверки результата.
- Спроектировал процесс Jira → планирование → реализация → проверка → ревью → PR с двумя режимами: безопасный dry-run (без изменений) и контролируемый реальный прогон.
- Ввёл проверки качества: тесты, изолированный sandbox, secret scan, ревью человеком, budget guard на расходы API.

## Как устроен процесс

Процесс разделён на безопасный dry-run и контролируемый реальный прогон — чтобы было видно: агенты не пушат код сами по себе, а работают внутри управляемого контура.

- Приём Jira-задачи с критериями приёмки.
- Планирование и предложение по реализации.
- Проверка в изолированном sandbox-окружении (без доступа к сети).
- Проверки качества и обязательное ревью человеком.
- Создание GitHub Pull Request.
- Пакет артефактов по итогам прогона.

## Проверки и контроль

Результат AI не считается доверенным, пока не пройдёт явные проверки:

- Обязательные тесты и lint.
- Запуск в sandbox без доступа к сети.
- Secret scan перед созданием PR.
- Ревью человеком.
- Budget guard на использование модели/API.
- Артефакты прогона: summary, статусы этапов, снимок метрик, логи sandbox и история по задаче.

## Метрики (snapshot)

| Метрика | Сигнал |
|---|---|
| Run success rate | доля успешных прогонов pipeline |
| Sandbox pass rate | доля прогонов, прошедших sandbox validation |
| Retry rate | частота повторных итераций после failed tests / review |
| Cost per run | ориентировочная стоимость одного прогона GenAI workflow |
| Stage cycle time | длительность отдельных этапов |
| Failure distribution | распределение ошибок по этапам |
| Lead time to PR | время от старта прогона до готового PR |
| **Validation snapshot** | **55 автотестов проходят, lint чистый; 150 run records по 11 issue-группам** |

Числа — portfolio validation evidence, а не production benchmark. Статус-репорт: [Status report (sample)](/evidence/pipeline-status-report.html).

## Risk Register

Для каждого риска заданы probability, impact, severity, owner, trigger, mitigation, contingency и review cadence. Ключевые:

| Риск | Митигация |
|---|---|
| Небезопасный или неработающий AI-сгенерированный код | обязательные тесты, Docker sandbox, reviewer approval, retry loop |
| Утечка секретов в сгенерированном коде | secret scan, отсутствие `.env` в sandbox, sanitization rules для evidence |
| Неконтролируемый real-run | dry-run по умолчанию, controlled GitHub Actions workflow, governance checks |
| Превышение бюджета на LLM API | budget guard, cost tracking, метрика cost per run |
| Неполные evidence artifacts | summary.json, sandbox logs, metrics snapshot, issue-level artifact history |

## Как строил (по фазам)

- **Прототип:** Python-скрипт — задача → Claude (structured output) → pytest в изоляции, до 3 итераций по ошибкам тестов.
- **Интеграции:** тонкие клиенты Jira и GitHub без AI (если связка не работает без AI — остальному доверять нельзя).
- **Агенты:** Planner (план + критерии приёмки), Coder (код + тесты), Reviewer (корректность, стиль, покрытие, safety).
- **Оркестратор:** полный цикл Jira → план → код → ревью-итерации → ветка и PR → ссылка обратно в Jira.
- **Аналитика:** прогон на наборе задач, сбор метрик, разбор провалов.

## Архитектура

- `agents/` — промпты, схемы и обёртки для planner, coder и reviewer.
- `integrations/` — клиенты Jira и GitHub.
- `orchestrator/` — основной цикл, retries, метрики и обработка ошибок.
- `tests/` — unit-тесты клиентов и логики оркестрации.
- `docs/` — charter, backlog, roadmap, DoD, risk register, quality gates, runbook, rollback playbook, evidence package, metrics, retro, decision log, demo cases.

## Осознанные ограничения

- Без параллельного исполнения.
- Без дополнительных агентов (DevOps, документация).
- Без production-контейнеризации в первой версии.
- Без попыток автоматизировать большие и расплывчатые задачи.

## Ретро

- **Сработало:** связал Jira → AI-этапы → sandbox validation → ревью → GitHub PR в единый процесс поставки; quality gates делают процесс управляемым; артефакты прогона объясняют его результат; документация покрывает scope, риски, метрики, откат и приёмку.
- **Улучшить дальше:** dashboard по метрикам; approval matrix для ролей PM / Tech Lead / Security / Operator; шаблон incident report; больше benchmark-прогонов; публичный обезличенный evidence-пакет.
- **Ключевой вывод:** ценность PM здесь не в том, чтобы «сделать AI-пайплайн», а в том, чтобы превратить технический эксперимент в понятный, измеримый и управляемый процесс поставки.

## Статус

Рабочий локальный прототип. Контур Jira → … → PR работает, артефакты прогонов сохраняются для аудита и ревью.

## Стек

Python, Claude API, Jira Cloud, GitHub API, GitHub Actions, pytest, Docker
