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
- [ ] `huk-proba` срабатывает по триггеру и снимается после первого успеха (`once: true`). НЕ проверено: headless-запуск не прошёл авторизацию → проверить руками: `claude --plugin-dir ./plugins/yunga` → `/yunga:huk-proba`.
- [ ] README прочитан посторонним человеком (без контекста сборки).

## Последующие релизы (когда появится содержимое) — из раздела 7.6

- [ ] `claude plugin validate . --strict` и `./plugins/yunga --strict` — чисто.
- [ ] Приватность и закон: стоп-лист (раздел 9) + шаблонный Grep + стоп-слова моделей (8.4) + агент-проверка.
- [ ] Вес: `claude plugin details` в пределах бюджета (~400 токенов честного веса).
- [ ] Evals безопасности (≥2.1.269) зелёные.
- [ ] Чистая учётка: `CLAUDE_CONFIG_DIR` (матрица — `docs/SURFACES-MATRIX.md`).
