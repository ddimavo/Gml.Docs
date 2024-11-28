Вот обновленная версия вашей документации:

# Установка из исходников GitHub

Инструкции по установке и настройке серверной части проекта для прочих операционных систем, не имеющих установщика

### Репозиторий на GitHub

Ссылка на репозиторий: [Gml.Backend](https://github.com/GamerVII-NET/Gml.Backend)

### Инструкция по установке

#### Шаг 1: Клонирование репозитория

Выполните следующую команду в вашем терминале:

<tabs>
    <tab title="Стабильная версия">
      <code-block lang="bash">
        git clone --recursive https://github.com/GamerVII-NET/Gml.Backend.git
      </code-block>
    </tab>
    <tab title="Последняя актуальная">
      <code-block lang="bash">
            git clone --recursive \
                      --branch develop https://github.com/GamerVII-NET/Gml.Backend.git
      </code-block>
    </tab>
</tabs>

#### Шаг 2: Переход в директорию проекта

Выполните следующую команду в вашем терминале:

```bash
cd Gml.Backend
```

#### Шаг 3: Настройка config файла

Отредактируйте или создайте файл `.env` в корне папки `Gml.Backend`

```yaml
# Пример уже настроенного .env

# UID (User Identifier) и GID (Group Identifier) используется
# для указания id пользователя и группы в Linux.
UID=0
GID=0

SECURITY_KEY=643866c80c46c909332b30600d3265803a3807286d6eb7c0d2e164877c809519
PROJECT_NAME=GmlBackendPanel
PROJECT_DESCRIPTION=
PROJECT_POLICYNAME=GmlServerPolicy
PROJECT_PATH=

  # Включить подключение к S3
S3_ENABLED=true
  # root пользователь панели управления
MINIO_ROOT_USER=GamerVII
  # root пароль панели управления
MINIO_ROOT_PASSWORD=waefawegferyjerthdrthrtrdthdr

  # Настройки внешнего доступа
  # Порты, на которых будут работать приложения
  # адрес консоли (:5009 или 10.2.0.1:5009)
MINIO_ADDRESS=:5009
  # Порт консоли (совпадает с записью выше)
MINIO_ADDRESS_PORT=5009
  # адрес панели (:5010 или 10.2.0.1:5010)
MINIO_CONSOLE_ADDRESS=:5010
  # Порт (совпадает с записью выше)
MINIO_CONSOLE_ADDRESS_PORT=5010
  # Web Api
PORT_GML_BACKEND=5000
  # Панель управления проектом
PORT_GML_FRONTEND=5003
  # Файловый сервис
PORT_GML_FILES=5005
  # Сервис скинов
PORT_GML_SKINS=5006

  # Микросервисы:
SERVICE_TEXTURE_ENDPOINT=http://gml-web-skins:8085
```

Отредактируйте или создайте файл `.env` в папке `src\Gml.Web.Client\.env`

```yaml
 # Адрес к Web Api
NEXT_PUBLIC_BASE_URL=http://localhost:5000
NEXT_PUBLIC_PREFIX_API=api
NEXT_PUBLIC_VERSION_API=v1
```

#### Шаг 5: Запуск проекта с использованием Docker

Выполните следующую команду в вашем терминале:

```bash
docker compose up -d --build
```

Пожалуйста, убедитесь, что Docker установлен и работает на вашем компьютере для выполнения этой команды.


После выполнения команды Docker загрузит необходимые образы и запустит проект.
После запуска проекта, вы сможете открыть его в браузере, используя следующие адреса:

## Инфраструктура

> Внимание! Начиная с версии 0.1.0-rc1 файлы серверной части вынесены в директорию
> установки [Подробнее](profiles-add-files.md)

<procedure title="Сервеная инфраструктура" id="inject-a-procedure">
    <step>
        <p>
            <span>WebApi</span>
            <a href="http://localhost:5000">http://localhost:5000</a>
        </p>
    </step>
    <step>
        <p>
            <span>Web Dashboard</span>
            <a href="http://localhost:5003">http://localhost:5003</a>
            <br/>
            <code>необходимо пройти предварительную регистрацию</code>
        </p>
    </step>
    <step>
        <p>
            <span>Web FileBrowser (Удален с версии 0.1.0-rc1 <a href="https://discord.com/channels/585873186512437248/1238063781028823090/1279787369490288652">Описание</a>)</span>
            <a href="http://localhost:5005">http://localhost:5005</a>
            <br/>
            <code>логин и пароль по умолчанию: admin:admin</code>
        </p>
    </step>
    <step>
        <p>
            <span>Minio S3 (Распределенный файловый сервер)</span>
            <a href="http://localhost:5010/">http://localhost:5010</a>
            <br/>
            <code>логин и пароль по умолчанию: minioadmin:minioadmin </code>
        </p>
    </step>
</procedure>

Кроме того, я заметил, что ваш `.env` файл изменился. Если требуется, пожалуйста, внесите соответствующие изменения в документацию.