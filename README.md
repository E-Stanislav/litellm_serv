# litell_serv

Прокси-сервер для управления LLM провайдерами с единой точкой входа и поддержкой дочерних API ключей.

## Возможности

- Единый API для нескольких LLM провайдеров
- Создание и управление дочерними API ключами
- Веб-UI для администрирования
- Лимитирование requests и budget
- Кэширование через Redis

## Провайдеры

| Провайдер | Модели |
|-----------|--------|
| OpenAI | GPT-4, GPT-4-Turbo, GPT-3.5-Turbo |
| Azure OpenAI | GPT-4, GPT-3.5-Turbo |
| Anthropic | Claude-3-Opus, Claude-3-Sonnet |
| Ollama | Llama3, Mistral |
| MiniMax | MiniMax-Text-01 |

## Быстрый старт

```bash
cp .env.example .env
nano .env  # указать ключи

docker-compose up -d
```

Откройте http://localhost:4000 и войдите с Master Key.

## Создание дочерних ключей

```bash
curl -X POST http://localhost:4000/key/generate \
  -H "Authorization: Bearer $MASTER_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "key_alias": "my-service",
    "duration": "30d",
    "models": ["gpt-4", "MiniMaxText-01"]
  }'
```

## API эндпоинты

| Метод | Путь | Описание |
|-------|------|----------|
| POST | `/key/generate` | Создать ключ |
| GET | `/key/info` | Информация о ключе |
| DELETE | `/key/delete` | Удалить ключ |
| GET | `/key/list` | Список ключей |
| POST | `/chat/completions` | Чат |

## Переменные окружения

| Переменная | Описание |
|------------|----------|
| `LITELLM_MASTER_KEY` | Главный ключ для администрирования |
| `OPENAI_API_KEY` | Ключ OpenAI |
| `MINIMAX_API_KEY` | Ключ MiniMax |
| `ANTHROPIC_API_KEY` | Ключ Anthropic |
| `AZURE_API_KEY` | Ключ Azure OpenAI |