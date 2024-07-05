# Публикация MacOS 

macOS приложения обычно распространяются в формате `.app` [application bundle](https://en.wikipedia.org/wiki/Bundle_%28macOS%29#macOS_application_bundles). Для работы .NET Core и Avalonia проектов в `.app` пакете необходимо выполнить несколько дополнительных шагов после публикации приложения.

С Avalonia у вас будет структура папок `.app`, которая выглядит следующим образом:

```Bash
MyProgram.app
|
----Contents\
    |
    ------_CodeSignature\ (содержит информацию о подписании кода)
    |     |
    |     ------CodeResources
    |
    ------MacOS\ (все ваши DLL файлы и т.д. -- выходные данные `dotnet publish`)
    |     |
    |     ---MyProgram
    |     |
    |     ---MyProgram.dll
    |     |
    |     ---Avalonia.dll
    |
    ------Resources\
    |     |
    |     -----MyProgramIcon.icns (файл значка)
    |
    ------Info.plist (содержит информацию о вашем пакете, идентификаторе, версии и т.д.)
    ------embedded.provisionprofile (файл с информацией о подписании)
```

Для получения дополнительной информации о `Info.plist`, смотрите [документацию Apple](https://developer.apple.com/documentation/bundleresources/information_property_list).

## Создание пакета приложения

Существует несколько вариантов создания структуры папок `.app`. Вы можете сделать это на любой операционной системе, так как `.app` файл - это просто набор папок, организованных в определенном формате, и инструменты не зависят от операционной системы. Однако, если вы создаете на Windows за пределами WSL, исполняемый файл может не иметь правильных атрибутов для выполнения на macOS -- возможно, вам придется выполнить `chmod +x` для выходного бинарного файла (выходные данные, сгенерированные `dotnet publish`) с Unix машины. Это бинарный выход, который попадает в папку `MyApp.app/Contents/MacOS/`, и его имя должно соответствовать `CFBundleExecutable`.

Структура `.app` зависит от правильного форматирования и содержания файла `Info.plist`. Используйте Xcode для редактирования `Info.plist`, так как он имеет автозаполнение для всех свойств. Убедитесь, что:

* Значение [`CFBundleExecutable`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundleexecutable) соответствует имени бинарного файла, сгенерированного `dotnet publish` -- обычно это то же имя, что и у вашего .dll файла, **без** `.dll`.
* [`CFBundleName`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundlename) установлено на отображаемое имя вашего приложения. Если это имя длиннее 15 символов, также установите [`CFBundleDisplayName`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundledisplayname).
* [`CFBundleIconFile`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundleiconfile) установлено на имя вашего `icns` файла значка (включая расширение).
* [`CFBundleIdentifier`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundleidentifier) установлено на уникальный идентификатор, обычно в формате обратного DNS -- например, `com.myapp.macos`.
* [`NSHighResolutionCapable`](https://developer.apple.com/documentation/bundleresources/information_property_list/nshighresolutioncapable) установлено на true (`<true/>` в `Info.plist`).
* [`CFBundleVersion`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundleversion) установлено на версию вашего пакета, например, 1.4.2.
* [`CFBundleShortVersionString`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundleshortversionstring) установлено на строку версии вашего приложения, видимую пользователю, например, `Major.Minor.Patch`.

Если вам нужно зарегистрировать протокол или ассоциации файлов - откройте plist файлы других приложений в папке Applications и изучите их поля.

Пример протокола:

```xml
  <key>CFBundleURLTypes</key>
  <array>
    <dict>
      <key>CFBundleURLName</key>
      <string>AppName</string>
      <key>CFBundleTypeRole</key>
      <string>Viewer</string>
      <key>CFBundleURLSchemes</key>
      <array>
        <string>i8-AppName</string>
      </array>
    </dict>
  </array>
```

Пример ассоциации файлов

```xml
  <key>CFBundleDocumentTypes</key>
  <array>
      <dict>
        <key>CFBundleTypeName</key>
        <string>Sketch</string>
        <key>CFBundleTypeExtensions</key>
        <array>
          <string>sketch</string>
        </array>
        <key>CFBundleTypeIconFile</key>
        <string>icon.icns</string>
        <key>CFBundleTypeRole</key>
        <string>Viewer</string>
        <key>LSHandlerRank</key>
        <string>Default</string>
      </dict>
  </array>
```

Дополнительная документация по возможным ключам `Info.plist` доступна [здесь](https://developer.apple.com/documentation/bundleresources/information_property_list/bundle_configuration).

Если в какой-то момент инструменты выдают ошибку о том, что ваш файл с активами не имеет цели для `osx-64`, добавьте следующие [идентификаторы среды выполнения](https://docs.microsoft.com/en-us/dotnet/core/rid-catalog) в верхнюю `<PropertyGroup>` в вашем `.csproj`:

```xml
<RuntimeIdentifiers>osx-x64</RuntimeIdentifiers>
```

Добавьте другие идентификаторы среды выполнения по мере необходимости. Каждый идентификатор должен быть разделен точкой с запятой (;).

### Примечания по созданию файлов значков

Этот тип значков можно создать не только на устройствах Apple, но также возможно на устройствах под управлением Linux.  
Вы можете найти больше информации о том, как это сделать, в этом посте в блоге:  
[Creating Mac OS X Icons (icns) on Linux](https://dentrassi.de/2014/02/25/creating-mac-os-x-icons-icns-on-linux/)

### Примечания по исполняемому файлу `.app`

Файл, который фактически исполняется macOS при запуске вашего `.app` пакета, **не будет** иметь стандартное расширение `.dll`. Если содержимое вашей папки публикации, которое попадает внутрь `.app` пакета, не имеет как `MyApp` (исполняемый файл), так и `MyApp.dll`, вероятно, что-то не генерируется правильно, и macOS, вероятно, не сможет запустить ваш `.app` правильно.

[Недавние изменения в способе распространения и нотаризации .NET Core на macOS](https://docs.microsoft.com/en-us/dotnet/core/install/macos-notarization-issues) привели к тому, что исполняемый файл `MyApp` (также называемый "app host" в связанной документации) не генерируется. **Вам нужно, чтобы этот файл был сгенерирован для правильной работы вашего `.app`.** Чтобы убедиться, что он генерируется, выполните одно из следующих действий:

* Добавьте следующее в ваш `.csproj` файл:

```xml
<PropertyGroup>
  <UseAppHost>true</UseAppHost>
</PropertyGroup>
```

* Добавьте `-p:UseAppHost=true` в вашу команду `dotnet publish`.

### dotnet-bundle

:::warning
[dotnet-bundle не поддерживается](https://github.com/egramtel/dotnet-bundle/issues/16#issuecomment-1365767804), но все же должен работать.

Рекомендуется использовать `net6-macos`, который обработает создание пакета.
:::

[dotnet-bundle](https://github.com/egramtel/dotnet-bundle) - это [NuGet пакет](https://www.nuget.org/packages/Dotnet.Bundle/), который публикует ваш проект, а затем создает `.app` файл для вас.

Сначала добавьте проект как `PackageReference` в ваш проект. Добавьте его через менеджер пакетов NuGet или добавив следующую строку в ваш `.csproj` файл:

```xml
<PackageReference Include="Dotnet.Bundle" Version="*" />
```

После этого вы можете создать ваш `.app`, выполнив следующую команду в командной строке:

```bash
dotnet restore -r osx-x64
dotnet msbuild -t:BundleApp -p:RuntimeIdentifier=osx-x64 -p:UseAppHost=true
```

Вы можете указать другие параметры для команды `dotnet msbuild`. Например, если вы хотите опубликовать в режиме release:

```bash
dotnet msbuild -t:BundleApp -p:RuntimeIdentifier=osx-x64 -property:Configuration=Release -p:UseAppHost=true
```

или если вы хотите указать другое имя приложения:

```bash
dotnet msbuild -t:BundleApp -p:RuntimeIdentifier=osx-x64 -p:CFBundleDisplayName=MyBestThingEver -p:UseAppHost=true
```

Вместо того чтобы указывать `CFBundleDisplayName` и прочие параметры в командной строке, вы также можете указать их в вашем файле проекта:

```xml
<PropertyGroup>
    <CFBundleName>AppName</CFBundleName> <!-- Определяет также имя файла .app -->
    <CFBundleDisplayName>MyBestThingEver</CFBundleDisplayName>
    <CFBundleIdentifier>com.example</CFBundleIdentifier>
    <CFBundleVersion>1.0.0</CFBundleVersion>
    <CFBundlePackageType>APPL</CFBundlePackageType>
    <CFBundleSignature>????</CFBundleSignature>
    <CFBundleExecutable>AppName</CFBundleExecutable>
    <CFBundleIconFile>AppName.icns</CFBundleIconFile> <!-- Будет скопирован из выходного каталога -->
    <NSPrincipalClass>NSApplication</NSPrincipalClass>
    <NSHighResolutionCapable>true</NSHighResolutionCapable>
</PropertyGroup>
```

По умолчанию `dotnet-bundle` поместит файл `.app` в ту же папку, что и выходные данные `publish`: `[директория проекта]/bin/{Конфигурация}/netcoreapp3.1/osx-x64/publish/MyBestThingEver.app`.

Для получения дополнительной информации о параметрах, которые можно передать, смотрите [документацию dotnet-bundle](https://github.com/egramtel/dotnet-bundle).

Если вы создали `.app` на Windows, убедитесь, что выполнили `chmod +x MyApp.app/Contents/MacOS/AppName` с Unix-машини. В противном случае приложение не запустится на macOS.

### Ручной метод

Сначала опубликуйте ваше приложение ([документация dotnet publish](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-publish)):

```bash
dotnet publish -r osx-x64 --configuration Release -p:UseAppHost=true
```

Here's the translated version of the detailed tutorial on creating and packaging a macOS application using Avalonia and .NET Core:

---

Создание `Info.plist` файла, добавление или изменение ключей по необходимости:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>CFBundleIconFile</key>
    <string>myicon-logo.icns</string>
    <key>CFBundleIdentifier</key>
    <string>com.identifier</string>
    <key>CFBundleName</key>
    <string>MyApp</string>
    <key>CFBundleVersion</key>
    <string>1.0.0</string>
    <key>LSMinimumSystemVersion</key>
    <string>10.12</string>
    <key>CFBundleExecutable</key>
    <string>MyApp.Avalonia</string>
    <key>CFBundleInfoDictionaryVersion</key>
    <string>6.0</string>
    <key>CFBundlePackageType</key>
    <string>APPL</string>
    <key>CFBundleShortVersionString</key>
    <string>1.0</string>
    <key>NSHighResolutionCapable</key>
    <true/>
</dict>
</plist>
```

Теперь можно создать структуру папки `.app`, как указано в начале этой страницы. Если вам нужен скрипт для автоматизации, вы можете использовать что-то подобное \(macOS/Unix\):

```bash
#!/bin/bash
APP_NAME="/путь/к/вашему/выходному/MyApp.app"
PUBLISH_OUTPUT_DIRECTORY="/путь/к/вашему/публикационному/выходу/netcoreapp3.1/osx-64/publish/."
# PUBLISH_OUTPUT_DIRECTORY должен указывать на выходной каталог вашей команды dotnet publish.
# Один из примеров: /путь/к/вашему/csproj/bin/Release/netcoreapp3.1/osx-x64/publish/.
# Если вы хотите изменить выходные каталоги, добавьте `--output /мой/каталог/пути` в вашу команду `dotnet publish`.
INFO_PLIST="/путь/к/вашему/Info.plist"
ICON_FILE="/путь/к/вашему/myapp-logo.icns"

if [ -d "$APP_NAME" ]
then
    rm -rf "$APP_NAME"
fi

mkdir "$APP_NAME"

mkdir "$APP_NAME/Contents"
mkdir "$APP_NAME/Contents/MacOS"
mkdir "$APP_NAME/Contents/Resources"

cp "$INFO_PLIST" "$APP_NAME/Contents/Info.plist"
cp "$ICON_FILE" "$APP_NAME/Contents/Resources/$ICON_FILE"
cp -a "$PUBLISH_OUTPUT_DIRECTORY" "$APP_NAME/Contents/MacOS"
```

Если вы создали `.app` на Windows, убедитесь, что выполняете `chmod +x MyApp.app/Contents/MacOS/AppName` из Unix-машины. В противном случае приложение не запустится в macOS.

## Подписание вашего приложения

После создания файла `.app` вам, вероятно, захочется подписать его, чтобы его можно было подтвердить и распространять пользователям без проблем с Gatekeeper. Нотаризация обязательна для приложений, распространяемых вне магазина приложений, начиная с macOS 10.15 \(Catalina\). Для успешной нотаризации вам нужно будет включить [усиленный режим](https://developer.apple.com/documentation/security/hardened_runtime?language=objc) и выполнить `codesign` для вашего `.app`.

Для этого потребуется компьютер Mac, так как нам нужно будет использовать инструмент командной строки `codesign` из Xcode.

### Запуск codesign и включение усиленного режима

Включение усиленного режима производится в том же шаге, что и подписание кода. Вам нужно подписать все файлы в пакете `.app` в папке `Contents/MacOS`, что проще сделать с помощью скрипта, так как файлов там много. Для подписания ваших файлов вам понадобится учетная запись разработчика Apple. Для нотаризации вашего приложения вам нужно будет выполнить следующие шаги с [сертификатом разработчика Apple](https://developer.apple.com/developer-id/), требующим платной подписки Apple.

Также вам нужно будет установить инструменты командной строки Xcode. Вы можете сделать это, установив Xcode и запустив его, или выполнить `xcode-select --install` в командной строке и следовать подсказкам для установки инструментов.

Для начала включите усиленный режим с [исключениями](https://developer.apple.com/documentation/security/hardened_runtime?language=objc), создав файл `MyAppEntitlements.entitlements`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.security.cs.allow-jit</key>
    <true/>
    <key>com.apple.security.automation.apple-events</key>
    <true/>
</dict>
</plist>
```

Затем выполните этот скрипт для всех операций подписи:

```bash
#!/bin/bash
APP_NAME="/путь/к/вашему/выходному/MyApp.app"
ENTITLEMENTS="/путь/к/вашему/MyAppEntitlements.entitlements"
SIGNING_IDENTITY="Developer ID: MyCompanyName" # соответствует названию сертификата в доступе к ключам Keychain

find "$APP_NAME/Contents/MacOS/"|while read fname; do
    if [[ -f $fname ]]; then
        echo "[INFO] Подписание $fname"
        codesign --force --timestamp --options=runtime --entitlements "$ENTITLEMENTS" --sign "$SIGNING_IDENTITY" "$fname"
    fi
done

echo "[INFO] Подписание файла приложения"

codesign --force --timestamp --options=runtime --entitlements "$ENTITLEMENTS" --sign "$SIGNING_IDENTITY" "$APP_NAME"
```

Часть `--options=runtime` в строке `codesign` активирует усиленный режим с вашим приложением. Поскольку [.NET Core может быть не полностью совместим с усиленным режимом](https://github.com/dotnet/runtime/issues/10562#issuecomment-503013071), мы добавляем некоторые исключения для использования компилированного JIT-кода и разрешения отправки событий Apple. Исключение для компилированного JIT-кода необходимо для запуска приложений Avalonia под усиленным режимом. Второе исключение для событий Apple используется для устранения ошибок, которые могут появляться в Console.app.

Примечание: Microsoft перечисляет [некоторые другие исключения усиленного режима](https://docs.microsoft.com/en-us/dotnet/core/install/macos-notarization-issues#default-entitlements), которые требуются для .NET Core. Единственное исключение, действительно необходимое для запуска приложения Avalonia, - `com.apple.security.cs.allow-jit`. Остальные могут представлять угрозу безопасности для вашего приложения. Используйте с осторожностью.

После подписания вашего приложения вы можете проверить, что оно подписано правильно, убедившись, что следующая команда не выводит ошибок:

```bash
codesign --verify --verbose /путь/к/MyApp.app
```

