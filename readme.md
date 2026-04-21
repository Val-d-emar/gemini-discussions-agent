# 🤖 Gemini Discussion Agent

[🇷🇺 Русский](#русский) | [🇬🇧 English](#english)

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

<a name="english"></a>

## 🇬🇧 English

This repository contains a **GitHub Action** that leverages the power of Gemini (via Google AI) to automatically analyze and respond to GitHub Discussions. The agent is capable of studying the context of an entire discussion thread and providing meaningful, reasoned answers to user questions.

## ✨ Features

- **Context Analysis:** Reads the discussion title, original post (OP), and all subsequent comments.
- **Gemini Integration:** Uses `gemini-cli` to interact with the Google AI API.
- **Flexible Configuration:** Ability to set a specific system prompt and response language.
- **Automation:** Triggered when the agent is mentioned (e.g., `@ai-agent-net`) in comments.
- **Model Selection:** Supports selecting a specific model via the `@model` tag in the comment.

## 🚀 Quick Start

To use this action in your repository, create a `.github/workflows/agent.yml` file:

```yaml
on:
  discussion_comment:
    types: [created, edited]
  discussion:
    types: [created]
permissions:
  discussions: write
  contents: read

jobs:
  gemini_respond:
    if: contains(github.event.comment.body, '@ai-agent')
    runs-on: ubuntu-latest
    steps:
      - name: Extract Model Tag
        id: extract_model
        shell: bash
        run: |
          MODEL=$(echo "$COMMENT_BODY" | head -n 1 | grep -oP '@model\s+\K[a-zA-Z0-9.-]+' || echo "auto")
          echo "model=$MODEL" >> $GITHUB_OUTPUT
        env:
          COMMENT_BODY: ${{ github.event.comment.body }}

      - uses: Val-d-emar/gemini-discussions-agent@v1
        with:
          gemini_api_key: ${{ secrets.GEMINI_API_KEY }}
          agent_prompt: "Analyze the discussion and provide a technically accurate response."
          agent_language: "English"
          agent_model: ${{ steps.extract_model.outputs.model }}
```

## ⚙️ Parameters (Inputs)

| Parameter          | Description                                     | Required | Default                            |
| :----------------- | :---------------------------------------------- | :------: | :--------------------------------- |
| `gemini_api_key` | Your Google Gemini API key (supports rotation). |   Yes   | —                                 |
| `github_token`   | GitHub API token (needs write scopes).          |    No    | `${{ github.token }}`            |
| `github_email`   | Email for git commits.                          |    No    | `${{ github.actor.email }}`      |
| `github_name`    | Name for git commits.                           |    No    | `ai-${{ github.actor.login }}`   |
| `agent_prompt`   | Instructions for the AI agent.                  |    No    | (see action.yml)                   |
| `agent_language` | The language in which the agent will respond.   |    No    | `English`                        |
| `agent_model`    | Model for the AI agent (pro, flash, auto, etc.) |    No    | flash                              |
| `comment_title`  | Header added to the generated response.         |    No    | `### 🤖 Gemini Discussion Agent` |

## 🛠 Features & Capabilities

- **Repository Operations:** The agent can read/write code, run tests, create branches, and open Pull Requests using `git` and `gh` CLI.
- **Project Awareness:** Automatically generates a Project Map to help the agent navigate your codebase efficiently.
- **Key Rotation:** Supports multiple comma-separated API keys to handle Free Tier limits.
- **Detailed Logging:** All tool executions and shell commands are logged under a collapsible spoiler in the discussion comment.

## 📄 License

This project is licensed under the[GNU General Public License v3.0 (GPL-3.0)](LICENSE).

---

<a name="русский"></a>

## 🇷🇺 Русский

Этот репозиторий содержит **GitHub Action**, который использует мощь нейросети Gemini (через Google AI) для автоматического анализа и ответов в GitHub Discussions. Агент способен изучать контекст всей ветки обсуждения и предоставлять осмысленные, аргументированные ответы на вопросы пользователей.

## ✨ Особенности

- **Анализ контекста:** Считывает заголовок обсуждения, сообщение (OP) и все комментарии.
- **Интеграция с Gemini:** Использует `gemini-cli` для взаимодействия с API Google.
- **Гибкая настройка:** Возможность задать системный промпт и язык ответа.
- **Автоматизация:** Триггерится при упоминании агента (например, `@ai-agent-net`) в комментариях.
- **Выбор модели:** Поддержка выбора модели через тег `@model` (например, `@ai-agent-net @model flash-lite ...`).
- **Оптимизация квот (Eco-mode):** Улучшенные системные инструкции для минимизации лишних вызовов инструментов и экономии квоты API.
- **Ротация API ключей:** Поддержка нескольких ключей (через запятую) для обхода лимитов Free Tier.
- **Обработка ошибок:** При возникновении ошибок (лимиты API или сбои инструментов) в обсуждение автоматически публикуется отчет с техническими логами под спойлером.

## 🚀 Быстрый старт

Для использования этого экшена в своем репозитории, создайте файл `.github/workflows/agent.yml`:

```yaml
on:
  discussion_comment:
    types: [created, edited]
  discussion:
    types: [created]

permissions:
  discussions: write
  contents: read

jobs:
  gemini_respond:
    if: contains(github.event.comment.body, '@ai-agent')
    runs-on: ubuntu-latest
    steps:
      - uses: Val-d-emar/gemini-discussions-agent@main
        with:
          # Можно указать несколько ключей через запятую для ротации
          gemini_api_key: ${{ secrets.GEMINI_API_KEY }}
          agent_prompt: "Проанализируй текущий вопрос пользователя в контексте всего обсуждения и дай аргументированный ответ."
          agent_language: "Russian"
          agent_model: "flash"
```

## ⚙️ Параметры (Inputs)

| Параметр   | Описание                                                                 | Обязательно | По умолчанию            |
| :----------------- | :------------------------------------------------------------------------------- | :--------------------: | :--------------------------------- |
| `gemini_api_key` | API ключ Gemini (поддерживает ротацию через запятую).                  |          Да          | —                                 |
| `github_token`   | Токен GitHub (требуются права на запись).                        |         Нет         | `${{ github.token }}`            |
| `github_email`   | Email для git коммитов.                                          |         Нет         | `${{ github.actor.email }}`      |
| `github_name`    | Имя для git коммитов.                                           |         Нет         | `ai-${{ github.actor.login }}`   |
| `agent_prompt`   | Инструкции для ИИ-агента.                                   |         Нет         | (см. action.yml)                 |
| `agent_language` | Язык, на котором агент будет отвечать.            |         Нет         | `English`                        |
| `agent_model`    | Модель для использования (pro, flash, auto и т.д.)      |         Нет         | flash                              |
| `comment_title`  | Заголовок, который будет добавлен к ответу.  |         Нет         | `### 🤖 Gemini Discussion Agent` |

## 🛠 Возможности

- **Операции с репозиторием:** Агент может читать/писать код, запускать тесты, создавать ветки и Pull Requests, используя `git` и `gh` CLI.
- **Понимание проекта:** Автоматическая генерация карты проекта (Project Map) для быстрой навигации по коду.
- **Ротация ключей:** Поддержка нескольких API ключей для обхода лимитов Free Tier.
- **Прозрачное логирование:** Все выполненные команды и вызовы инструментов логируются под спойлером в ответе.

## 📄 Лицензия

Этот проект распространяется под лицензией [GNU General Public License v3.0 (GPL-3.0)](LICENSE).
