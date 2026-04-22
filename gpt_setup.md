# Настройка OmniRoute для GPT (OpenAI)

## 1. Получение API ключа OpenAI

1. Перейдите на https://platform.openai.com/api-keys
2. Войдите в аккаунт OpenAI
3. Нажмите **Create new secret key**
4. Скопируйте и сохраните ключ (он показывается только один раз!)

## 2. Настройка OmniRoute

### 2.1. Запуск OmniRoute

```bash
omniroute
```

### 2.2. Добавление провайдера OpenAI

1. Выбираем **Провайдеры** → **OpenAI**
2. Вставляем API ключ
3. Сохраняем настройки

### 2.3. Выбор модели

Доступные модели GPT:
- `gpt-4o` - самая новая и быстрая модель GPT-4
- `gpt-4-turbo` - GPT-4 Turbo
- `gpt-4` - стандартная GPT-4
- `gpt-3.5-turbo` - быстрая и дешевая модель

## 3. Использование с приложениями

### Параметры подключения:

- **API Endpoint**: `http://localhost:20128/v1`
- **API Key**: ваш ключ OpenAI
- **Model**: например, `gpt-4o`

### Пример для curl:

```bash
curl http://localhost:20128/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_OPENAI_KEY" \
  -d '{
    "model": "gpt-4o",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```

### Пример для Python:

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_OPENAI_KEY",
    base_url="http://localhost:20128/v1"
)

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "user", "content": "Hello!"}
    ]
)

print(response.choices[0].message.content)
```

## 4. Переключение между моделями

OmniRoute позволяет легко переключаться между:
- Claude (Anthropic)
- GPT (OpenAI)
- Gemini (Google)
- Другими провайдерами

Просто измените параметр `model` в запросе!

---

## Готово! 🎉

Теперь вы можете использовать GPT через OmniRoute.
