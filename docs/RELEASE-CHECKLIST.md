# Чек-лист релиза (авторам)

Источник — DESIGN-v1.md, раздел 12, абзац «Этап 0 — исполнимо» (и 7.6 «Проверка курса авторами» — для будущих этапов).

## Этап 0 — каркас (эта версия)

- [x] `claude --version` ≥ 2.1.260; `git --version` установлен. (17.09.2026: 2.1.273, git 2.55.0)
- [x] `claude plugin validate . --strict` из корня репозитория (маркетплейс) — exit 0.
- [x] `claude plugin validate ./plugins/yunga --strict` — exit 0.
- [x] `claude plugin details yunga@kubrik` — базовая линия веса записана (7 скиллов, 2 агента): always-on ≈204 токена; при вызове: course-scout ≈150, course-checker ≈110, huk-proba ≈70, pomogi ≈60, start/karta/otkat/pauza ≈50, slovo ≈30.
- [~] Grep приватности по каркасу — частично: имена, e-mail, личные пути, названия сторонних моделей — пусто; полный шаблон 7.6 п.2 (телефоны, длинные числа, домены, токены) — до публикации.
- [x] `version` в `plugin.json` — 0.1.0 (первый каркас, повышать не с чего).
- [x] `CHANGELOG.md` обновлён.
- [x] Установка на чистом `CLAUDE_CONFIG_DIR` (изолированная временная папка, DC\env-vars.md:394): `/plugin marketplace add ./` → `/plugin install yunga@kubrik` → `/yunga:start` виден; настройки автора не тронуты. (add/install — успех; плагин скопирован в `plugins/cache/kubrik/yunga/0.1.0/` → после правок переустанавливать; `/yunga:*` видны в сессии с `--plugin-dir`.)
- [~] `huk-proba` срабатывает по триггеру и снимается после первого успеха (`once: true`). **Проверено руками 17.09.2026** (CLI 2.1.273, Windows, Git Bash; `claude --plugin-dir ./plugins/yunga --setting-sources project,local --debug-file <лог>`): до вызова скилла хук не срабатывает; после `/yunga:huk-proba` в логе `Registered 1 hooks from skill 'yunga:huk-proba'`, на `echo test` видна строка `PreToolUse:Bash says: yunga huk-proba: хук сработал`, кириллица цела; **`once: true` не снял хук** — на следующей команде shell он сработал снова (оба запуска в логе `success`, записи о снятии нет). Вывод хука переведён с обычного `echo` на JSON `systemMessage`: обычный stdout PreToolUse ученик не видит (DC hooks.md:810, :953).
- [ ] README прочитан посторонним человеком (без контекста сборки).

## Этап 0.5 — установка и поверхность Desktop (P1, P4, P5)

Источник — DESIGN-v1.md: 7.4 (пути Д1–Д5), 10.1 (P1, P4, P5), 12. **Где:** отдельный компьютер с Windows и Claude Desktop, без Claude Code CLI и по возможности без git — как у ученика (решение заказчика 17.09.2026; машина автора не чистая, Desktop не работает с изолированным `CLAUDE_CONFIG_DIR`). **Кто что делает:** автор — руками по шагам ниже; Claude — готовит источник и промпты, после прогона читает снимки и транскрипты, заполняет итоги в этом файле.

### 0.5-0. Подготовка (Claude)
- [ ] Шаблонный Grep приватности каркаса целиком (7.6 п.2) — пусто.
- [ ] Публичный remote `https://github.com/<владелец>/yunga` (владелец — В20) — только после Grep и явного «да» заказчика; `owner.name`, `author`, LICENSE — на владельца.
- [ ] Запасной источник, если remote ещё нет: zip репо → распаковать на тестовом компьютере в `C:\yunga-src\` (латиница, без пробелов).
- [ ] **Снимок для Claude** после каждого шага — одна вставка в PowerShell тестового компьютера (`$s` — имя шага): копирует настройки, файлы плагинов и транскрипты только тестовых папок `*yunga*`; учётные данные не копируются.
  ```powershell
  $s='P4'; $o="C:\yunga-otchet\$s"; New-Item -ItemType Directory -Force "$o\projects" | Out-Null; Copy-Item "$env:USERPROFILE\.claude\settings.json","$env:USERPROFILE\.claude\plugins\*.json" $o -ErrorAction SilentlyContinue; Get-ChildItem "$env:USERPROFILE\.claude\projects" -Directory -Filter '*yunga*' -ErrorAction SilentlyContinue | Copy-Item -Destination "$o\projects" -Recurse
  ```
  Папку `C:\yunga-otchet\` после прогона передать Claude.

### P4 — slash-команды маршрута в Desktop (можно и на машине автора)
Автор: пустая папка `%USERPROFILE%\yunga-p4` (и рядом пустая `yunga-p4-extra`) → новая локальная сессия Desktop в ней, режим «Ask permissions» → команды по порядку, в открывшихся окнах ничего не менять → снимок `P4`. Claude по транскрипту ставит итог: ✅ работает · ⛔ `isn't available in this environment` · ↪ другое (что именно).

| # | Ввести | Оговорка | Итог |
|---|---|---|---|
| 1 | `/status` | показанную версию Claude Code — в итог | |
| 2–5 | `/model` · `/effort` · `/context` · `/usage` | без аргументов | |
| 6–7 | `/btw что такое контекст?` · `/recap` | | |
| 8–14 | `/doctor` · `/memory` · `/mcp` · `/hooks` · `/agents` · `/skill-doctor` · `/reload-plugins` | ничего не менять и не переподключать | |
| 15 | `/add-dir <полный путь к yunga-p4-extra>` | | |
| 16 | `/init` | создаст `CLAUDE.md` в тестовой папке — это нормально | |
| 17 | `/config` | откроются Settings — закрыть без изменений | |
| 18 | `/fewer-permission-prompts` | предложит записать правила — отказаться | |
| 19 | `/deep-research` | только есть ли в меню `/`; не запускать — расходует лимит | |
| 20–22 | `/rewind` · `/resume` · `/compact` | | |
| 23 | `/clear` | последней | |

Дополнительно к P4 (одобрение `.mcp.json`, ask-хук PreModelSwitch при смене модели меню, Notification) — Claude готовит файлы для тестовой папки отдельно, после основной таблицы.

### P5 — правка settings после частичного чтения
Автор: папка `%USERPROFILE%\yunga-zhurnal-p5` (имитация журнала), режим «Ask permissions», **две сессии**: модель Haiku 4.5 (всегда требует Read — DC tools-reference.md:221) и модель по умолчанию у ученика. Если `~/.claude/settings.json` нет — сначала в отдельной сессии попросить создать его с содержимым `{ "$schema": "https://json.schemastore.org/claude-code-settings.json", "permissions": { "allow": [], "deny": [] }, "cleanupPeriodDays": 30 }` в несколько строк (ключ `cleanupPeriodDays` — ниже 3-й строки). В каждой из двух сессий — промпт ниже; запрос на чтение — «Yes», **запрос на запись — «No»** (файл не меняется) → снимок `P5`.
```text
Прочитай файл ~/.claude/settings.json инструментом Read только с offset 1 и limit 3 — больше этот файл ничем не читай. Затем инструментом Edit замени в нём "cleanupPeriodDays": 30 на "cleanupPeriodDays":  30 (два пробела). Больше ничего не делай. Если Edit вернёт ошибку — перескажи её дословно.
```
Claude по транскрипту: спросило ли чтение файла вне папки; Edit — ошибка «файл не прочитан» (частичное чтение не засчитано) или дошёл до запроса на запись (засчитано или не требуется); какая модель.

### P1 — пути установки Д1–Д5 (порядок — от дешёвого к меняющему машину)
`<источник>` — HTTPS-адрес репо (`https://github.com/<владелец>/yunga.git`) или, без remote, `C:\yunga-src`. **Сброс между путями:** удалить плагин (+ → Plugins → Manage plugins), убрать маркетплейс `kubrik` как получится (записать как), вернуть `settings.json` из копии → снимок `sbros-<путь>`.

1. **Д3** — в поле ввода `/plugin`, затем `/plugin marketplace add <источник>`: доступно или `isn't available…`? Если доступно — `/plugin install yunga@kubrik` → виден ли `/yunga:start` в меню `/` → снимок `D3`.
2. **Д5 [кандидат, только осмотр]** — Customize в боковой панели → Plugins: можно ли добавить свой плагин (ссылка, zip) и какие варианты предлагает. **Ничего не загружать:** плагин, включённый для аккаунта claude.ai, синхронизируется во все сессии этого аккаунта (DC plugins-reference.md:421-428).
3. **Д2** — новая сессия в папке `%USERPROFILE%\yunga-zhurnal`, промпт-договор:
   ```text
   Хочу поставить курс yunga. Сначала покажи, что именно допишешь в ~/.claude/settings.json, и сделай копию этого файла рядом (settings.json.bak). Допиши только после моего «да»: в extraKnownMarketplaces маркетплейс kubrik с источником git и url <HTTPS-адрес репо>. Больше ничего не меняй.
   ```
   (без remote: источник `directory`, `path` — `C:/yunga-src`; по DC он только для разработки — settings-reference.md). → «да» → сосчитать диалоги разрешений → + → Plugins → Add plugin: виден ли yunga без перезапуска? Нет — полностью закрыть Desktop (значок в трее → Quit) и открыть → виден? → Install → `/yunga:start` в меню `/`? → снимок `D2`.
4. **Д1** — Views → Terminal → `claude --version`. Команды нет — это итог. Есть — `claude plugin marketplace add <источник>` → `claude plugin install yunga@kubrik` → в сессии `/reload-plugins` → `/yunga:start` виден? → снимок `D1`.
5. **Д4 (последним: ставит CLI)** — окно PowerShell: `irm https://claude.ai/install.ps1 | iex` → в том же окне `claude --version` (есть ли сразу; нет — новое окно) → `claude plugin marketplace add <источник>` → `claude plugin install yunga@kubrik` → полностью закрыть и открыть Desktop → `/yunga:start` виден? → снимок `D4`.
6. После remote: поднять `version`, отправить в remote → проверить обновление плагина в Desktop.

| Путь | Без терминала | Сработал | Нужен перезапуск Desktop | Диалогов разрешений | Примечание |
|---|---|---|---|---|---|
| Д1 | нет | | | | |
| Д2 | да | | | | |
| Д3 | да | | | | |
| Д4 | нет (мост) | | | | |
| Д5 | да | осмотр | | | |

**Критерий P1** (DESIGN-v1 7.4): флагман ставит курс сам по README через Д2, Д3 или Д5 без терминала; Д4 — запасной мост, допустим для пилота; если без терминала не вышло — В19: не выпускать до пилота №1. Ветки CLI с git/без на Windows и CLI на macOS — после флагмана.

## Последующие релизы (когда появится содержимое) — из раздела 7.6

- [ ] `claude plugin validate . --strict` и `./plugins/yunga --strict` — чисто.
- [ ] Приватность и закон: стоп-лист (раздел 9) + шаблонный Grep + стоп-слова моделей (8.4) + агент-проверка.
- [ ] Вес: `claude plugin details` в пределах бюджета (~400 токенов честного веса).
- [ ] Evals безопасности (≥2.1.269) зелёные.
- [ ] Чистая учётка: `CLAUDE_CONFIG_DIR` (матрица — `docs/SURFACES-MATRIX.md`).
