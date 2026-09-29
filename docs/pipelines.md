# Пайплайны команды агентов

Оркестратор — `team-lead`.

## Матрица

| task_type | pipeline |
|-----------|----------|
| hotfix | software-engineer → qa-engineer (± code-reviewer) |
| feature | software-engineer → qa-engineer → code-reviewer → docs-writer |
| feature_auth | как feature + **ib-auditor** (qa ∥ reviewer ∥ ib-auditor после diff) |
| infra | devops-engineer (± software-engineer) → ib-auditor → docs-writer |
| docs_only | docs-writer |
| consult | guru-1c / guru-typescript (+ сводка team-lead) |

`devops-engineer` подключается, если в scope есть Docker, K8s, CI, Terraform, Ansible или деплой.

## Правила оркестрации

1. Классифицировать `task_type`.
2. Вести сессию в `.agents_log/sessions/<session_id>.json` (ключи/enum/пути — EN; `summary` и criteria — RU).
3. Делегировать через Task или `/имя-агента` с брифом: `session_id`, путь к шине, DoD этапа, `evidence_path`.
4. После ответа субагента проверять `steps` и evidence; обновлять `current_stages` / `status`.
5. `ib-auditor` включает только team-lead по матрице (auth / API / данные / `feature_auth`).
6. Для задач по стеку 1С: запросить версию платформы, определить версию конфигурации и передать в бриф `guru-1c`.
7. Эскалация владельцу: продукт, стоимость, необратимые риски, security `critical` / `high`.
8. `status=done` → перенос файла сессии в `.agents_log/archive/`.

## Коммиты и шина

- Коммиты: только владелец или `software-engineer`.
- Шина: только `.agents_log/` в корне репо.
- Не коммитить `.agents_log/` и секреты.
