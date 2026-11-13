# Инструменты из видео: как подключить

Эти три инструмента работают **на вашем компьютере**: им нужен локальный фоновый процесс или прокси. Устанавливайте их там, где вы запускаете Claude Code (терминал или десктоп-приложение), а не в облачной сессии claude.ai/code.

## 1. claude-mem — память между сессиями
Записывает, что делал Claude, и подставляет нужный контекст в следующие сессии.

Требования: Node.js 20.19+ или 22.12+. Bun и uv установщик поставит сам.

```bash
npx claude-mem install
```
Или внутри Claude Code:
```
/plugin marketplace add thedotmack/claude-mem
/plugin install claude-mem
```
После установки перезапустите Claude Code. Настройки лежат в `~/.claude-mem/settings.json`.
⚠️ Команда `npm install -g claude-mem` **не** подключает плагин. При установке выберите провайдера памяти: ваш план Anthropic, свой ключ OpenRouter/Gemini или их платный облачный observer с пробным периодом, для которого нужна карта.

## 2. Headroom — сжатие контекста (экономия токенов)
Сжимает выводы инструментов, логи и файлы, прежде чем они попадут в модель. Работает локально, ключ не нужен.

Требования: Python 3.11+ и [uv](https://docs.astral.sh/uv/).

```bash
uv tool install --python 3.13 "headroom-ai[all]"
headroom wrap claude      # запускает Claude Code через локальный прокси
```
Проверка: `headroom doctor`. Статистика экономии: `headroom perf`. Отключить: `headroom unwrap claude`.

## 3. OmniRoute — шлюз к разным моделям
Локальный шлюз (`http://localhost:20128/v1`): переключает запросы между провайдерами, когда у одного кончается лимит.

```bash
npm install -g omniroute       # или Docker-образ diegosouzapw/omniroute
omniroute configure claude     # интерактивная настройка Claude Code
# или разовый запуск:
omniroute run claude --model openai/gpt-5.4
```
Нужны ключ OmniRoute (Dashboard → Endpoints) и аккаунт хотя бы у одного провайдера моделей.
⚠️ Через OmniRoute Claude Code будет работать на моделях других компаний, а не на Claude. Проверьте условия этих провайдеров.

> Headroom и OmniRoute оба работают как прокси перед Claude Code. Включать их одновременно не советую: начните с одного.

## 4–5. Claude Code Setup и Task Observer
Это плагины из каталога Anthropic Directory. Их ставят кнопкой в claude.ai (карточка плагина) или командой `/plugin` в Claude Code.
- **claude-code-setup** — анализирует проект и советует, какие хуки, скиллы, MCP-серверы и субагенты добавить.
- **Task Observer** — превращает ваши поправки в правила в `CLAUDE.md`. Команды: `/learn`, `/preferences`.
