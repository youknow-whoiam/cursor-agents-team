# Cursor Agents Team

Мультиагентная команда для Cursor: **team-lead** оркестрирует, субагенты пишут код, тесты, инфру, ревью и консультации. Шина сессий — `.agents_log/` в корне рабочего репо.

## Роли

| Агент | Уровень | Режим | Зона |
|-------|---------|-------|------|
| [team-lead](agents/team-lead.mdc) | T3 | оркестратор | классификация задач, делегирование, шина |
| [software-engineer](agents/software-engineer.md) | T1 | write | фичи и багфиксы |
| [devops-engineer](agents/devops-engineer.md) | T1 | write | CI/CD, Docker/K8s, IaC, деплой |
| [qa-engineer](agents/qa-engineer.md) | T1 | test-only | тесты по acceptance_criteria |
| [code-reviewer](agents/code-reviewer.md) | T2 | read-only | ревью диффа |
| [ib-auditor](agents/ib-auditor.md) | T3 | read-only | security: auth / API / данные |
| [docs-writer](agents/docs-writer.md) | T1 | docs | README, API docs, ADR |
| [guru-1c](agents/guru-1c.md) | T2 | consult | консультации 1С |
| [guru-typescript](agents/guru-typescript.md) | T2 | consult | консультации TS/React |

## Пайплайны

| task_type | pipeline |
|-----------|----------|
| hotfix | software-engineer → qa-engineer (± code-reviewer) |
| feature | software-engineer → qa-engineer → code-reviewer → docs-writer |
| feature_auth | feature + **ib-auditor** (параллельно после diff) |
| infra | devops-engineer (± software-engineer) → ib-auditor → docs-writer |
| docs_only | docs-writer |
| consult | guru-* (+ сводка team-lead) |

Подробнее: [docs/pipelines.md](docs/pipelines.md).

## Установка в Cursor

1. Скопируйте файлы из `agents/` (кроме `team-lead.mdc`) в `~/.cursor/agents/`.
2. Положите `team-lead.mdc` в `.cursor/rules/` целевого репозитория (`alwaysApply: true`).
3. В рабочих репо заведите `.agents_log/` (не коммитьте).
