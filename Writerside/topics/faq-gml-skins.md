# Сервис текстур minecraft

Мы постарались максимально упростить загрузку скинов для вашего игрового проекта, достаточно знать реальный адрес скина.
Для игровых проектов майнкрафт - это ссылка на текстуры в личном кабинете, если у вас нет сайта или проекта с личным
кабинетом,
вы можете использовать [TLauncher](https://tlauncher.org/ru/catalog/skins/nickname/) или [Ely.by](https://ely.by)

## Использование готовых сервисов

На просторах интернета вы могли уже встречать скины по никам, и если у вас нет ни сайта, ни системы скинов - используйте
их.
Для их работы ничего не нужно. Вы можете использовать один из следующих вариантов:

<tabs>
<tab title="TLauncher">
<code-block lang="yaml">
# Скины
https://tlauncher.org/upload/all/nickname/{userName}.png
# Плащи
https://tlauncher.org/upload/all/cloaks/{userName}.png
</code-block>
</tab>
<tab title="T-Monitoring">
<code-block lang="yaml">
# Скины
https://tmonitoring.com/uploads/catalog/skins/nickname/{userName}.png
# Плащи
https://tmonitoring.com/uploads/catalog/capes/{userName}.png
</code-block>
</tab>
<tab title="SkinMc (Только скины)">
<code-block lang="yaml">
# Скины
https://skinmc.net/api/v1/skins/uuid/{userUuid}
# Плащи
https://skinmc.net/api/v1/cloaks/uuid/{userUuid}
</code-block>
</tab>
<tab title="ru-minecraft.ru (Устарело)">
<code-block lang="yaml">
# Скины
https://ru-minecraft.ru/mcskins/skins/{userName}.png
# Плащи
https://ru-minecraft.ru/mcskins/cloaks/{userName}.png
</code-block>
</tab>
<tab title="minecraft-inside.ru (Устарело)">
<code-block lang="yaml">
# Скины
https://minecraft-inside.ru/skins/nick/{userName}.html?download
# Плащи
https://minecraft-inside.ru/skins/cloaks/{userName}.html?download
</code-block>
</tab>
</tabs>

## Свой сервис скинов

> **Важно!** Данных вариант подходит для тех проектов, у которых есть сайт с возможностью загрузки скинов или плащей

Если вы имеете личный кабинет, вы должны знать реальный адрес текстуры, по каждому пользователю, например им может
выступать
примерно такой адрес:

<tabs>
<tab title="Пример ссылки">
Скины
<code-block lang="Text">
https://mc.recloud.tech/cabient/skins/GamerVII.png
</code-block> Плащи
<code-block>
https://mc.recloud.tech/cabient/cloaks/GamerVII.png
</code-block>
</tab>
<tab title="Пример ответа">
<img src="faq-gml-skins-1.png" alt="Пример скина" /> По такому адресу мы будем получать реальную текстуру скина следующего вида
</tab>
</tabs>

Именно такие ссылки нужно указать на странице "Интеграции" -> "Сервис скинов"
![faq-gml-skins-2.png](faq-gml-skins-2.png) {width="580"}

> **Важно!** Вместо ника вы можете использовать **UUID** или **Ник игрока**
> Замените его в соответствии со следующими правилами
> ```
> {userName} - Ник пользователя
> {userUuid} - Uuid пользователя
> ```
> По итогу ссылки будут выглядеть примерно следующим образом:
> ```
> https://mc.recloud.tech/cabient/skins/{userName}.png
> https://mc.recloud.tech/cabient/cloaks/{userName}.png
> ```

Пример в системе:
![faq-gml-skins.png](faq-gml-skins.png) {width="580"}

## Устранение проблем с текстурами

1. Проверьте, что файлы скинов и плащей скачиваются, по ссылке, которую вы указали на странице "Интеграции" -> "Сервис
   скинов"
2. Убедитесь по какому протоколу возвращаются текстуры по HTTP или HTTPS и используйте его везде
3. Используйте проксирование для Gml.Web.Api: c 5000 портом, для того чтобы грузить скины по HTTPS
4. Если вы обращаетесь к серверу по IP или localhost - используйте ТОЛЬКО HTTP! В настройках укажите его же!
   ![faq-gml-skins-3.png](faq-gml-skins-3.png) {width="580"}
5. В случае неправильной конфигурации - проверьте раздел Настройки панели и выполните действия, которые требует панель
   ![faq-gml-skins-4.png](faq-gml-skins-4.png) {width="580"}
6. Убедитесь, что в [настройках лаунчера](https://gml-launcher.github.io/Gml.Docs/installation-launcher.html#pckyza_12)
   так же указан верный протокол, по которому работает связка с Gml Api