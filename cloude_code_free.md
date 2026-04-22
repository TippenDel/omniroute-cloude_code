# Установка Claude Code через OmniRoute

## 1. Проверка версии Node.js

Убедитесь, что установлена версия `v22.22.2 (LTS)`:

```bash
node -v
```

## 2. Установка OmniRoute

### 2.1. Настройка директории для глобальных npm-пакетов

```bash
mkdir -p ~/.npm-global
npm config set prefix ~/.npm-global
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.zshrc
```

### 2.2. Применение изменений

```bash
source ~/.zshrc
```

### 2.3. Установка OmniRoute

```bash
npm install -g omniroute
```

### 2.4. Проверка установки

```bash
which omniroute
omniroute --help
```

## 3. Установка Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

## 4. Настройка подключения к Kiro AI

### 4.1. Запуск OmniRoute

```bash
omniroute
```

### 4.2. Выбор провайдера

Выбираем **Провайдеры** → **Kiro AI**

![KiroAI](./images/KiroAI.png)

### 4.3. Добавление соединения

Нажимаем **Добавить соединение** → **AWS Builder ID**

![AWS Builder ID](./images/AWS_Builder_ID.png)

Нажимаем на ссылку и проходим верификацию

![Connection KiroAI](./images/Connection_KiroAI.png)

### 4.4. Создание API ключа

Возвращаемся в OmniRoute. Переходим в **Менеджер API** → **Создать ключ API**

![APIkey](./images/APIkey.png)

Придумайте имя (например: `CloudeCode`)

> **⚠️ ВАЖНО!** Обязательно сохраните ключ в надежном месте!

### 4.5. Получение параметров подключения

Вам понадобятся следующие параметры:

**YOUR_URL** - URL эндпоинта:

![URL](./images/URL.png)

**YOUR_MODEL** - название модели:

![model](./images/model.png)

## 5. Запуск Claude Code

### Вариант 1: Скрипт запуска

Создайте файл `start-claude.sh` и замените параметры:
- `YOUR_KEY` - API ключ, который вы сохранили
- `YOUR_URL` - URL из OmniRoute
- `YOUR_MODEL` - название модели
- `_YOU_` - ваше имя пользователя macOS

```bash
#!/bin/zsh

# API Key
export ANTHROPIC_AUTH_TOKEN="YOUR_KEY"

# OmniRoute endpoint
export ANTHROPIC_BASE_URL="YOUR_URL"

# Указание модели
export ANTHROPIC_MODEL="YOUR_MODEL"

# Отключение experimental betas
export CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS=1

exec /Users/_YOU_/.npm-global/bin/claude
```

Сделайте файл исполняемым:

```bash
chmod +x ~/Desktop/start-claude.sh
```

Запуск:

```bash
# Сначала запустите OmniRoute
omniroute

# Затем в новом терминале запустите Claude Code
~/Desktop/start-claude.sh
```

### Вариант 2: Автоматический запуск (двойной клик)

Создайте файл `start-claude.command` для запуска двойным кликом:

```bash
#!/bin/zsh

# =====================================
# OmniRoute + Claude Code
# Автоматический запуск
# =====================================

echo "🚀 Запускаю OmniRoute..."

omniroute > ~/omniroute.log 2>&1 &
OMNI_PID=$!

echo "⏳ Ожидание запуска OmniRoute..."

TRIES=0

until curl -s http://localhost:20128/v1/models > /dev/null 2>&1
do
    sleep 1
    TRIES=$((TRIES + 1))

    if [ $TRIES -gt 30 ]; then
        echo "❌ OmniRoute не запустился"
        echo "Лог: ~/omniroute.log"
        read -n 1 -s -r -p "Нажмите любую клавишу..."
        exit 1
    fi
done

echo "✅ OmniRoute запущен"

export ANTHROPIC_AUTH_TOKEN="YOUR_KEY"
export ANTHROPIC_BASE_URL="http://localhost:20128/v1"
export ANTHROPIC_MODEL="kr/claude-sonnet-4.5"
export CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS=1

echo "🤖 Запускаю Claude Code..."

exec claude
```

Сделайте файл исполняемым:

```bash
chmod +x ~/Desktop/start-claude.command
```

Теперь можно запускать двойным кликом по файлу!

---

## Готово! 🎉

Теперь вы можете использовать Claude Code через OmniRoute с Kiro AI.
