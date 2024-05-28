# Собственная авторизация

#### Предустановленные типы

На данный момент Gml.Backend поддерживает несколько типов предустановленных типов авторизации:

| Тип авторизации | Описание                                                                                                        |
|-----------------|-----------------------------------------------------------------------------------------------------------------|
| Undefined       | **Запрещена** на любую авторизацию со стороны лаунчера                                                          |
| Any             | **Разрешена** любая авторизация со стороны лаунчера                                                             |
| DataLifeEngine  | Скрипт авторизации для CMS <br/>[DataLifeEngine](https://dle-news.ru)                                           |
| Azuriom         | Авторизация через CMS <br/>[Azuriom](https://github.com/Azuriom/Azuriom)                                        |
| EasyCabinet     | Авторизация через систему личного кабинета <br/>[Aurora EasyCabinet](https://github.com/AuroraTeam/EasyCabinet) |
| UnicoreCMS      | Авторизация через CMS систему UnicoreCMS                                                                        |

#### Как работает авторизация?

![Frame 39230.png](integrations-auth-custom-1.png)

#### 1. Лаунчер - Gml.Backend
<tabs>
    <tab title="cURL">
        <code-block lang="curl">
curl --location 'http://localhost:5000/api/v1/integrations/auth/signin' \
--header 'Content-Type: application/json' \
--data '{
    "Login": "ЛОГИН",
    "Password": "ПАРОЛЬ"
}'
        </code-block>
    </tab>
    <tab title="JavaScript">
        <code-block lang="javascript">
const myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");
const raw = JSON.stringify({
"Login": "ЛОГИН",
"Password": "ПАРОЛЬ"
});
const requestOptions = {
method: "POST",
headers: myHeaders,
body: raw,
redirect: "follow"
};
fetch("http://localhost:5000/api/v1/integrations/auth/signin", requestOptions)
.then((response) => response.text())
.then((result) => console.log(result))
.catch((error) => console.error(error));
        </code-block>
    </tab>

</tabs>

#### 2. Gml.Backend - Ваш сайт
<tabs>
    <tab title="cURL">
        <code-block lang="curl">
curl --location 'http://ВАШ_АДРЕС' \
--header 'Content-Type: application/json' \
--data '{
    "Login": "ЛОГИН",
    "Password": "ПАРОЛЬ"
}'
        </code-block>
    </tab>
    <tab title="JavaScript">
        <code-block lang="javascript">
const myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");
const raw = JSON.stringify({
"Login": "ЛОГИН",
"Password": "ПАРОЛЬ"
});
const requestOptions = {
method: "POST",
headers: myHeaders,
body: raw,
redirect: "follow"
};
fetch("http://ВАШ_АДРЕС", requestOptions)
.then((response) => response.text())
.then((result) => console.log(result))
.catch((error) => console.error(error));
        </code-block>
    </tab>
    <tab title="Пример вашего ответа">
        <code class="code">200</code> - Успешная авторизация
        <code-block lang="json">
        {
            "Login": "GamerVII",
            "UserUuid": "c07a9841-2275-4ba0-8f1c-2e1599a1f22f",
            "Message": "Успешная авторизация"
        }
        </code-block>
        <code class="code">404</code> - Пользователь не найден
        <code-block lang="json">
        {
            "Message": "Пользователь не найден"
        }
        </code-block>
        <code class="code">401</code> - Ошибка аутентификации
        <code-block lang="json">
        {
            "Message": "Неверный логин или пароль"
        }
        </code-block>
        <p>* Обратите внимание, что не обязательно заполнять тело ответа, достаточно вернуть StatusCode (Обязательно)
        Поля <code>Message</code> и <code>UserUuid</code> будут сгенерированы автоматически</p>
    </tab>  

</tabs>

#### Собственная авторизация

Есть возможность реализовать свою, кастомную авторизацию через Endpoint. 
Для этого нужно на стороне вашего сервиса реализовать [Endpoint](https://en.wikipedia.org/wiki/Web_API#Endpoints)

1. Выберите в панель управления раздел:
```Интеграции``` -> ```Аутентификация``` -> ```Аутентификация``` -> ```Собственная аутентификация```
2. Вставьте ссылку на реализованный endoint авторизации ```Gml.Backend - Ваш сайт```