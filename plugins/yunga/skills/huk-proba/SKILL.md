---
name: huk-proba
description: Учебный хук — единственный хук плагина, показывает ученику, как срабатывает датчик на команде
disable-model-invocation: true
hooks:
  PreToolUse:
    - matcher: "Bash|PowerShell"
      hooks:
        - type: command
          command: 'echo "yunga huk-proba: хук сработал"'
          once: true
---

Каркас этапа 0: урок ещё не написан.

Назначение: единственный хук плагина (M12) — показать ученику живое срабатывание PreToolUse до первой команды shell, затем снять себя (`once: true`).

После загрузки выполни в shell команду `echo test`, чтобы показать срабатывание учебного хука.
