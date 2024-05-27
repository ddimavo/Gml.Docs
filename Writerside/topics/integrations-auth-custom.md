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
fetch("http://local:5000/api/v1/integrations/auth/signin", requestOptions)
.then((response) => response.text())
.then((result) => console.log(result))
.catch((error) => console.error(error));
        </code-block>
    </tab>
    <tab title="JavaScript">
        <code-block lang="javascript">

        </code-block>
    </tab>

</tabs>


#### Собственная авторизация

Есть возможность реализовать свою, кастомную авторизацию через Endpoint. Для этого нужно на стороне вашего сервиса реализовать [Endpoint](https://en.wikipedia.org/wiki/Web_API#Endpoints)