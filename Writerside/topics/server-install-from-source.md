# Установка из исходников GitHub

Добро пожаловать в раздел серверной части Gml.Backend. Здесь вы найдете инструкции по установке и настройке серверной части проекта.

### Репозиторий на GitHub

Ссылка на репозиторий: [Gml.Backend](https://github.com/GamerVII-NET/Gml.Backend)

### Инструкция по установке

#### Шаг 1: Клонирование репозитория

Выполните следующую команду в вашем терминале:

<tabs>
    <tab title="Последняя актуальная">
      <code-block lang="bash">
            git clone --recursive \
                      --branch dev https://github.com/GamerVII-NET/Gml.Backend.git
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

```
# API Endpoint для запросов
# Необходимо заменить на свой адрес и порт,
# на котором расположело API
# порт указывается в переменной PORT_GML_BACKEND
API_URL=http://localhost:5000                              

# База данных Для GLITCHTIP (создаётся автоматически)
POSTGRES_USER=admin # Пользователь базы данных
POSTGRES_PASSWORD=admin # Пароль базы данных
POSTGRES_DB=project  # Пользователь базы данных

# Настройки Sentry (Сервис логирования ошибок) Домен либо IP с адресом
GLITCHTIP_DOMAIN=http://localhost:5007
# Ключ ОБЯЗАТЕЛЬНО 32 СИМВОЛА (Из букв и цифр)
# Можно воспользоваться консольной командой: openssl rand -hex 32
GLITCHTIP_SECRET_KEY=secretKey
ADMIN_EMAIL=admin@localhost

# Настройки внешнего доступа 
# Порты, на которых будут работать приложения
PORT_GML_BACKEND=5000   # Web Api
PORT_GML_FRONTEND=5003  # Панель управления проектом
PORT_GML_FILES=5005     # Файловый сервис
PORT_GML_SKINS=5006     # Сервис скинов
PORT_GML_SENTRY=5007    # Панель управления ошибками
```

Отредактируйте или создайте файл `.env` в папке `src\Gml.Web.Client\.env`

```
NEXT_PUBLIC_BASE_URL=http://localhost:5000 # Адрес к Web Api
NEXT_PUBLIC_PREFIX_API=api
NEXT_PUBLIC_VERSION_API=v1
```

#### Шаг 4: Настройка appsettings.json (Опционально)

Отредактируйте или создайте файл `appsettings.json` в папке `src\Gml.Web.Api\src\Gml.Web.Api`

<tabs>
    <tab title="Пример">
      <code-block lang="json">
        {
          "Logging": {
            "LogLevel": {
              "Default": "Information",
              "Microsoft.AspNetCore": "Warning"
            }
          },
          "ServerSettings": {
            "ProjectName": "GmlServer1", # Название проекта
            "ProjectDescription": "Project Description", # Описание проекта
            "ProjectPath": "", # Заполнить, если нужно переименовать папку
            "ProjectVersion": "1.0.0-alpha", # Версия приложения (Authlib)
            "SecretKey": "SecretKey", # Секретный ключ (openssl rand -hex 32)
            "SkinDomains": [ # Домены и поддомены, откуда берутся скины
                "localhost",
                "recloud.tech",
                ".recloud.tech"
            ],
            "PolicyName": "GmlPolicy" # Наименование CORS политики
          },
          "ConnectionStrings": {
            "SQLite": "Data Source=data.db" # Путь к локальной базе данных
          }
        }
        </code-block>
    </tab>
    <tab title="Чистый файл">
        <code-block lang="json">
        {
          "Logging": {
            "LogLevel": {
              "Default": "Information",
              "Microsoft.AspNetCore": "Warning"
            }
          },
          "ServerSettings": {
            "ProjectName": "",
            "ProjectPath": "",
            "ProjectVersion": "",
            "SkinDomains": [],
            "ProjectDescription": "",
            "SecretKey": "",
            "PolicyName": ""
          },
          "ConnectionStrings": {
            "SQLite": ""
          }
        }
        </code-block>
    </tab>
</tabs>

#### Шаг 5: Запуск проекта с использованием Docker

Выполните следующую команду в вашем терминале:

```bash
docker compose up
```

Пожалуйста, убедитесь, что Docker установлен и работает на вашем компьютере для выполнения этой команды.

После выполнения команды Docker загрузит необходимые образы и запустит проект.
После запуска проекта, вы сможете открыть его в браузере, используя следующие адреса:

## Инфраструктура

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
            <span>Web FileBrowser</span>
            <a href="http://localhost:5005">http://localhost:5005</a>
            <br/>
            <code>логин и пароль по умолчанию: admin:admin</code>
        </p>
    </step>
</procedure>


