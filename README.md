<div align="center">

# 🧭 My Chrome MCP

**Дайте AI-агенту «руки» у вашому Chrome — за одне доручення.**

ChatGPT для комп'ютера · Claude Desktop · будь-який агент з MCP

![MCP](https://img.shields.io/badge/MCP-stdio-059669) ![chrome-devtools-mcp](https://img.shields.io/badge/chrome--devtools--mcp-1.12.0-1C1917) ![Chrome](https://img.shields.io/badge/Chrome-144%2B-4285F4) ![Node.js](https://img.shields.io/badge/Node.js-20.19%2B-339933) ![Мова](https://img.shields.io/badge/мова-українська-FFD500)

</div>

---

## Що це

Готовий «рецепт» для підключення офіційного MCP-сервера Google **[chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp)** у режимі **«мій браузер»** (`--autoConnect`).

Агент працює у вашому звичайному Chrome: з вашими вкладками й входами в акаунти. Тому може те, чого не може окремий браузер: подивитися кабінет, CRM, пошту, де ви вже увійшли.

```
 Ви ──доручення──▶ Агент (ChatGPT, Claude…) ──MCP──▶ chrome-devtools-mcp ──▶ ВАШ Chrome
                                                                      ▲
                                            «Allow remote debugging?» — натискаєте ви
```

> Цей репозиторій містить лише інструкції й фрагменти налаштувань. Коду тут немає: сам сервер — пакет Google з npm, версія зафіксована.

## ⚠️ Прочитайте перед встановленням

| | |
|---|---|
| 🔓 **Агент бачить ваш Chrome** | усі відкриті вкладки, профілі, входи в акаунти |
| 🙋 **«Allow» натискаєте лише ви** | Chrome питає дозвіл при кожному новому підключенні. Якщо агент намагається натиснути сам — зупиніть його |
| 🧹 **Закрийте зайве** | робочі профілі, банк, пошту з чужими даними — до запуску |
| 🧪 **Почніть з окремого профілю** | створіть у Chrome новий профіль «Агент» і працюйте в ньому |
| 🛑 **«Always allow» не натискайте** | на картках дозволу в ChatGPT/Claude — лише «Allow once» |

Хочете безпечніший варіант без ваших входів — ставте той самий сервер з `--isolated` (окремий чистий Chrome).

## Що потрібно

- **Google Chrome 144+** (`chrome://version`)
- **Node.js 20.19+** — Windows: `winget install --id OpenJS.NodeJS.LTS -e`; Mac: інсталятор `.pkg` з [nodejs.org](https://nodejs.org)
- Агент з підтримкою локальних MCP: **ChatGPT для комп'ютера** (режим Work), **Claude Desktop** або інший

## 🚀 Спосіб 1 — попросіть агента встановити

Відкрийте ChatGPT для комп'ютера → режим **Work** → **Ask for approval** і вставте:

```text
Встанови MCP-сервер за інструкцією https://github.com/prodelt/My_chrome_mcp_elt (файл AGENTS.md).
Додай лише цей сервер, інших налаштувань не змінюй. Потім скажи, що мені натиснути.
```

Агент попросить один дозвіл на зміну налаштувань → **Allow once**. Далі зробіть кроки з розділу [Після встановлення](#-після-встановлення).

## 🛠 Спосіб 2 — вручну

### ChatGPT для комп'ютера (Windows / Mac)

Settings → **Integrations → Plugins** → вкладка **MCPs** → **Add** → **Add MCP server** → STDIO:

| Поле | Значення |
|---|---|
| Name | `chrome-mine` |
| Command to launch | `npx` |
| Arguments (кожен окремим полем, «+ Add argument») | `-y` · `chrome-devtools-mcp@1.12.0` · `--autoConnect` · `--no-usage-statistics` |

→ **Save** → **Restart**. Те саме у файлі — [`config/codex-config.toml`](config/codex-config.toml).

### Claude Desktop

Settings → **Developer** → **Edit Config** → додайте блок з [`config/claude_desktop_config.json`](config/claude_desktop_config.json) у `mcpServers` (інші сервери не чіпайте) → повністю перезапустіть Claude.

## ✅ Після встановлення

1. **Chrome:** відкрийте `chrome://inspect/#remote-debugging` → увімкніть **Allow remote debugging for this browser instance**.
2. **Агент:** повністю закрийте й відкрийте (ChatGPT: File → Quit). Перевірка в чаті ChatGPT: `/mcp` → `chrome-mine Enabled`.
3. **Перший запуск** — доручення з назвою сервера:

   ```text
   Використай chrome-mine: відкрий bank.gov.ua у новій вкладці і скажи курс євро на сьогодні. Інших вкладок не чіпай.
   ```

4. Дві картки: в агенті **Allow once** → у Chrome **«Allow remote debugging?» → Allow** (діалог може сховатися за вікном агента — клацніть Chrome на панелі задач; чекає ≈60 с).

## 🧯 Якщо не працює

| Що бачите | Що робити |
|---|---|
| У `/mcp` сервера немає | повністю перезапустіть агента |
| `npx not found`, помилка запуску | `node -v` у терміналі; поставте Node.js; перезапустіть ПК |
| `Connection closed` (Windows 10) | Command: `cmd`, Arguments: `/c` · `npx` · `-y` · `chrome-devtools-mcp@1.12.0` · `--autoConnect` · `--no-usage-statistics` |
| Агент «не бачить» Chrome | перевірте галочку на `chrome://inspect/#remote-debugging` і натисніть Allow у Chrome |
| Перший старт довгий | зачекайте хвилину → Restart (`startup_timeout_sec = 60` у config.toml) |

## 🗑 Видалити

ChatGPT: Settings → MCPs → `chrome-mine` → видалити (або приберіть блок `[mcp_servers.chrome-mine]` з `~/.codex/config.toml`). Claude: приберіть `chrome-mine` з конфігу. У Chrome вимкніть галочку на `chrome://inspect/#remote-debugging`.

## Порівняння

| | Вбудований браузер агента | `--isolated` (окремий Chrome) | **`--autoConnect` (цей рецепт)** |
|---|---|---|---|
| Ставити | нічого | Node.js + MCP | Node.js + MCP + галочка в Chrome |
| Ваші входи | ні | ні | **так** |
| Для чого | читати публічні сайти | клікати, заповнювати, перевіряти сайти | кабінети, CRM, пошта — під наглядом |
| Ризик | низький | низький | **високий** |

## Джерела й ліцензія

- Сервер: [ChromeDevTools/chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp) (Google, Apache-2.0) — прапорці `--autoConnect`, `--isolated`, `--no-usage-statistics`.
- MCP у ChatGPT для комп'ютера: [developers.openai.com/codex/mcp](https://developers.openai.com/codex/mcp).
- Перевірено 05.11.2026: ChatGPT для комп'ютера (Free, Windows 11), chrome-devtools-mcp 1.12.0, Chrome з `--autoConnect`. На Mac і в Claude Desktop — за документацією.
- Цей репозиторій (інструкції й фрагменти) — [MIT](LICENSE). Матеріал курсу «AI для бізнесу».
