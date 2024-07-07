# Запуск в Docker

На данной странице продемонстрированы примерные настройки для запуска игрового сервера в docker-контейнере, на примере ядра PAPER 1.20.4

## 1. Подготовка окружения
Перед началом установки, убедитесь что у вас установлены следующие программы:

- Docker ([Официальный сайт](https://www.docker.com/products/docker-desktop/))

## 2. Конфигурационный файл
Создайте в удобном для вас месте файл с наименованием ```docker-compose.yml``` со следующим содержимым

<tabs>
<tab title="Оригинал">
<code-block lang="yaml">
version: '3.8'
services:
    mc:
        image: itzg/minecraft-server
        tty: true
        stdin_open: true
        ports:
            - "25565:25565"
        environment:
            # Версия ядра
            TYPE: PAPER
            # Версию сервера
            VERSION: 1.20.4
            EULA: "TRUE"
            ONLINE_MODE: "TRUE"
            ENFORCE_SECURE_PROFILE: "FALSE"
            # Объем оперативной памяти
            MEMORY: 4G
            JVM_OPTS: "-javaagent:{ПУТЬ_ДО_authlib-injector-1.2.5.jar}=https://{АДРЕС_К_ВАШЕМУ_GML_API}/api/v1/integrations/authlib/minecraft -Dauthlibinjector.debug"
        volumes:
            # Создаём volume для сохранения данных сервера
            - ./data:/data
</code-block>
</tab>
<tab title="Пример заполненного файла">
<code-block lang="yaml">
version: '3.8'
services:
    mc:
        image: itzg/minecraft-server
        tty: true
        stdin_open: true
        ports:
            - "25565:25565"
        environment:
            # Версия ядра
            TYPE: PAPER
            # Версию сервера
            VERSION: 1.20.4
            EULA: "TRUE"
            ONLINE_MODE: "TRUE"
            ENFORCE_SECURE_PROFILE: "FALSE"
            # Объем оперативной памяти
            MEMORY: 4G
            JVM_OPTS: "-javaagent:libraries/authlib-injector-1.2.5-alpha-1.jar=https://localhost:5000/api/v1/integrations/authlib/minecraft -Dauthlibinjector.debug"
        volumes:
            # Создаём volume для сохранения данных сервера
            - ./data:/data
</code-block>
<p>
Обратите внимание, что файл в данном случае загружен в папку libraries вашего игрового сервера
</p>
</tab>
</tabs>

## 3. Загрузка библиотек
Создайте папку ```data/libraries``` и загрузите туда [authlib injector ](https://github.com/Gml-Launcher/Gml.Authlib.Injector/releases/tag/authlib-injector-1.2.5-alpha-1)

Конечная иерархия должна выглядеть следующим образом:
```
📁 data
|-- 📁 libraries
    |-- 📄 authlib-injector-1.2.5-alpha-1.jar
📄 docker-compose.yml

```

## 4. Первый запуск
После пропишите в консоли команду:
```Bash
docker compose up
```

Если вы все сделали правильно, то в консоли будет примерно следующее
![server-minecraft-docker](server-minecraft-docker.png)