# Версии Claude Code — что от какой версии зависит

Минимум для курса — **Claude Code ≥ 2.1.260** (самая поздняя функция основного маршрута — `/reload-plugins` в Desktop). Прогон авторов — на 2.1.273. Источник — DESIGN-v1.md §7.5; «DC\файл.md:строка» — снимок документации от 17.09.2026.

| Функция | С версии | Модуль | Если версия ниже → замена в уроке |
|---|---|---|---|
| Установка плагина активирует его сразу | 2.1.221 (discover-plugins.md:325) | Установка | Новая сессия |
| Archive-маркетплейс без git | 2.1.224 (plugin-marketplaces.md:455) | Установка | Git-путь |
| `/reload-plugins` в Desktop | 2.1.260 (discover-plugins.md:420) | Установка | Перезапуск Desktop |
| `/auto-mode-setup` | 2.1.228, Windows — 2.1.233 (auto-mode-config.md:175) | M02 | Пропустить шаг |
| SessionStart resume-поля (`context_tokens`), PreModelSwitch | 2.1.251 (hooks.md:1157, :3110) | M12 | Хук капсулы работает без слежения; ручной `/context` вместо PreModelSwitch |
| `/skill-doctor` | 2.1.252 (skills.md:817) | M08 | `/doctor` (skills.md:1079) |
| Проверка `< file` по правилам чтения (Read-deny) | 2.1.257 (permissions.md:302) | M02 | Граница объясняется шире, без демонстрации этого случая |
| Межсессионные сообщения (cross-session messaging) | 2.1.224, Windows — 2.1.234 (cross-session-messaging.md:10) | M13 | Рассказ вместо демонстрации |
| `blockReadsOutsideWorkingDirectories` | 2.1.257 (settings-reference.md:1540) | M00, M02 | Объяснение пропускается |

## Как узнать версию
- **CLI:** `claude --version` (cli-reference.md:137); обновление — `claude update` (:22).
- **Desktop:** Help → About / Claude → About Claude (desktop.md:931-932); обновляется сам при запуске на macOS и Windows (:950); вручную — Check for Updates (:185).
- **Версия встроенного Claude Code (любая поверхность):** `/status` (commands.md:142); в Desktop — см. `procedures/surface-steps.md`.

## Что делает `start`, если версия ниже минимума
Не блокирует курс полностью: называет, как обновиться (ссылка на официальную установку — `reference/surfaces.md`), помечает в `progress.md` шаги из таблицы выше как «недоступно, версия ниже N» и продолжает маршрут без них, где это не критично. Критично для основного маршрута — только `/reload-plugins` (сама установка); ниже 2.1.260 курс сначала предлагает обновиться.

**Переменные окружения (только CLI, для авторов и любопытных):** `DISABLE_AUTOUPDATER` — выключает фоновые обновления; `DISABLE_UPDATES` — и ручные тоже (env-vars.md:408, :428). В курсе не используются, в `na-vyrost.md` — по желанию.
