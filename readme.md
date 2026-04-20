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
| `gemini_api_key` | Your Google Gemini API key.                     |   Yes   | —                                 |
| `github_token`   | GitHub API token (needed for posting comments). |    No    | `${{ github.token }}`            |
| `agent_prompt`   | Instructions for the AI agent.                  |    No    | (see action.yml)                   |
| `agent_language` | The language in which the agent will respond.   |    No    | `English`                        |
| `agent_model`    | Model for the AI agent (pro, flash, auto, etc.) |    No    | `auto`                           |
| `comment_title`  | Header added to the generated response.         |    No    | `### 🤖 Gemini Discussion Agent` |

## 🛠 How It Works

1. The action is triggered by a new comment in Discussions.
2. The full history of the current discussion is fetched via the GraphQL API.
3. Data is formatted into a `discussion_context.txt` file.
4. Gemini analyzes the context and generates a response based on the provided prompt.
5. The response is posted as a new comment in the same discussion thread.

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
- **Выбор модели:** Поддержка выбора конкретной модели через тег `@model` в тексте комментария.

## 🚀 Быстрый старт

Для использования этого экшена в своем репозитории, создайте файл `.github/workflows/agent.yml`:

```yaml
on:
  discussion_comment:
    types: [created, edited]

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
          github_token: ${{ secrets.AI_TOKEN_PAT }}
          agent_prompt: "Проанализируй текущий вопрос пользователя в контексте всего обсуждения и дай аргументированный ответ."
          agent_language: "Russian"
          agent_model: ${{ steps.extract_model.outputs.model }}
```

## ⚙️ Параметры (Inputs)

| Параметр   | Описание                                                                 | Обязательно | По умолчанию            |
| :----------------- | :------------------------------------------------------------------------------- | :--------------------: | :--------------------------------- |
| `gemini_api_key` | Ваш API ключ для Google Gemini.                                        |          Да          | —                                 |
| `github_token`   | Токен для доступа к API GitHub (нужен для записи). |         Нет         | `${{ github.token }}`            |
| `agent_prompt`   | Инструкции для ИИ-агента.                                   |         Нет         | (см. action.yml)                 |
| `agent_language` | Язык, на котором агент будет отвечать.            |         Нет         | `English`                        |
| `agent_model`    | Модель для использования (pro, flash, auto и т.д.) |         Нет         | `auto`                           |
| `comment_title`  | Заголовок, который будет добавлен к ответу.  |         Нет         | `### 🤖 Gemini Discussion Agent` |

## 🛠 Как это работает

1. Экшен запускается при создании комментария в Discussions.
2. С помощью GraphQL API выгружается полная история текущего обсуждения.
3. Данные форматируются в файл `discussion_context.txt`.
4. Gemini анализирует контекст и генерирует ответ согласно заданному промпту.
5. Ответ публикуется новым комментарием в ту же ветку обсуждения.

## 📄 Лицензия

Этот проект распространяется под лицензией [GNU General Public License v3.0 (GPL-3.0)](LICENSE).
