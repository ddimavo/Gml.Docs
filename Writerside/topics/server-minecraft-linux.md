# Запуск в Linux

### 1. Установка JDK

Скачайте и установите **Java 17** или более новую версию.

### 2. Загрузка ядра сервера

Скачайте серверное ядро, например, [PAPER 1.20.4](https://papermc.io/downloads).

### 3. Настройка authlib injector

1. Скачайте
   [authlib-injector](https://github.com/Gml-Launcher/Gml.Authlib.Injector/releases/tag/authlib-injector-1.2.5-alpha-1)-
2. Поместите его в папку с сервером.

### 4. Запуск сервера

Создайте файл `start.bat` (для Windows) или `start.sh` (для Linux/MacOS) с таким содержимым:

#### Windows (start.bat)

```tex
@echo off
java -Xmx4G -Xms4G -javaagent:authlib-injector-1.2.5-alpha-1.jar=https://localhost:5000/api/v1/integrations/authlib/minecraft -jar paper-1.20.4.jar nogui
pause
```

#### Linux/MacOS (start.sh)

```bash
#!/bin/bash
java -Xmx4G -Xms4G -javaagent:authlib-injector-1.2.5-alpha-1.jar=https://localhost:5000/api/v1/integrations/authlib/minecraft -jar paper-1.20.4.jar nogui
```

Не забудьте сделать скрипт исполняемым на Linux/MacOS:

```bash
chmod +x start.sh
```

Запустите скрипт. Сервер начнёт работать.

> Важно!
> Не оставляйте `localhost:5000`, если сервер будет использоваться другими игроками или развёрнут на удалённой машине!
> В параметре `-javaagent` замените `https://localhost:5000/api/v1/integrations/authlib/minecraft` на адрес вашего API,
> где развернута интеграция **authlib injector**.

Если ваш сервер доступен по адресу `https://api.example.com`, то строка должна выглядеть следующим образом:

```bash
-javaagent:libraries/authlib-injector-1.2.5-alpha-1.jar=https://api.example.com/api/v1/integrations/authlib/minecraft -Dauthlibinjector.debug
```