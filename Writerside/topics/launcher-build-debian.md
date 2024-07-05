# Публикация для Debian / Ubuntu

Лаунчер Gml.Launcher написан с использованием технологии [Avalonia](https://avaloniaui.net).

Avalonia для Linux может быть запущена на большинстве дистрибутивов Linux с помощью двойного щелчка по исполняемому файлу или через терминал. 
Тем не менее, для лучшего пользовательского опыта рекомендуется установить программу, чтобы пользователи могли запускать её через ярлык на рабочем столе,
существующий в средах рабочего стола, таких как GNOME и KDE, или через командную строку, добавив программу в PATH.

Приложения для дистрибутивов, связанных с Debian и Ubuntu, упаковываются в файлы `.deb`, которые можно установить с помощью команды `sudo apt install ./your_package.deb`.

## Руководство

В этом руководстве мы будем использовать инструмент `dpkg-deb` для компиляции вашего пакета `.deb`.

### 1) Организация файлов программы в папке подготовки

Пакеты Debian имеют следующую базовую структуру:

```
./staging_folder/
├── DEBIAN
│   └── control # файл управления пакетом
└── usr
    ├── bin
    │   └── myprogram # стартовый скрипт
    ├── lib
    │   └── myprogram
    │       ├── libHarfBuzzSharp.so # библиотека Avalonia
    │       ├── libSkiaSharp.so # библиотека Avalonia
    │       ├── other_native_library_1.so
    │       ├── myprogram_executable # основной исполняемый файл
    │       ├── myprogram.dll 
    │       ├── my_other_dll.dll 
    │       ├── ... # все файлы, сгенерированные dotnet publish
    └── share
        ├── applications
        │   └── MyProgram.desktop # ярлык на рабочем столе
        ├── icons
        │   └── hicolor
        │       ├── ... # иконки различных разрешений (необязательно)
        └── pixmaps
            └── myprogram.png # основная иконка приложения
```

Значение каждой папки:

* `DEBIAN`: содержит файл `control`.
* `/usr/bin/`: содержит стартовый скрипт (рекомендуется для запуска программы через командную строку).
* `/usr/lib/myprogram/`: содержит все файлы, сгенерированные `dotnet publish`.
* `/usr/share/applications/`: папка для ярлыка на рабочем столе.
* `/usr/share/pixmaps/` и `/usr/share/icons/hicolor/**`: папки для иконок приложения.

> Папка `/usr/share/icons/hicolor/**` является необязательной, так как иконка вашего приложения, вероятно, 
> отобразится на рабочем столе и без этих изображений, однако рекомендуется их наличие для лучшего разрешения.

### 2) Создание файла `control`

Файл `control` размещается в папке `DEBIAN`.

Этот файл описывает общие аспекты вашей программы, такие как её имя, версия, категория, зависимости, сопровождающий, архитектура процессора и лицензии. Более подробное описание всех возможных полей в файле можно найти в [документации Debian](https://www.debian.org/doc/debian-policy/ch-controlfields.html).

> Не беспокойтесь о заполнении всех возможных полей, большинство из них не обязательны. Это руководство предназначено для создания "достаточно хорошего" пакета Debian.

Зависимости .NET можно перечислить, выполнив команду `apt show dotnet-runtime-deps-8.0` (суффикс меняется для других версий .NET); они будут отображены в строке, начинающейся с *Depends: ...*. Зависимости Avalonia включают: `libx11-6, libice6, libsm6, libfontconfig1`. В целом, все зависимости .NET и Avalonia обязательны, плюс любые другие, специфичные для вашего приложения.

Ниже приведен простой пример файла `control`.

```
Package: myprogram
Version: 3.1.0
Section: devel
Priority: optional
Architecture: amd64
Installed-Size: 68279
Depends: libx11-6, libice6, libsm6, libfontconfig1, ca-certificates, tzdata, libc6, libgcc1, libgssapi-krb5-2, libstdc++6, zlib1g, libssl1.0.0 | libssl1.0.2 | libssl1.1 | libssl3, libicu | libicu72 | libicu71 | libicu70 | libicu69 | libicu68 | libicu67 | libicu66 | libicu65 | libicu63 | libicu60 | libicu57 | libicu55 | libicu52
Maintainer: Ken Lee <kenlee@outlook.com>
Homepage: https://github.com/kenlee/myprogram
Description: Это MyProgram, отличный для выполнения X.
Copyright: 2022-2024 Ken Lee <kenlee@outlook.com>
```

### 3) Создание стартового скрипта

Этот шаг рекомендуется по двум причинам: во-первых, для уменьшения сложности ярлыка на рабочем столе, а во-вторых, для того, чтобы ваше приложение можно было запустить из терминала.

Имя файла стартового скрипта должно быть предпочтительно `myprogram` (без расширения `.sh`), чтобы при вводе "myprogram" в терминале пользователи могли запустить ваше приложение.

Файл **myprogram_executable** обычно имеет то же имя, что и его .NET проект, например, если ваш Avalonia .csproj проект называется *MyProgram.Desktop*, то основной исполняемый файл, сгенерированный dotnet publish, будет `MyProgram.Desktop`.

Пример стартового скрипта:

```
#!/bin/bash
# используем exec, чтобы скрипт-обертка не оставался как отдельный процесс
# "$@" для передачи аргументов командной строки приложению
exec /usr/lib/myprogram/myprogram_executable "$@"
```

### 4) Создание ярлыка на рабочем столе

Файл ярлыка на рабочем столе соответствует [спецификации freedesktop](https://specifications.freedesktop.org/desktop-entry-spec/desktop-entry-spec-latest.html#recognized-keys). Вики Arch Linux также содержит [полезную информацию](https://wiki.archlinux.org/title/Desktop_entries).

Ниже приведен пример файла ярлыка на рабочем столе.

```
[Desktop Entry]
Name=MyProgram
Comment=MyProgram, отличный для выполнения X
Icon=myprogram
Exec=myprogram
StartupWMClass=myprogram
Terminal=false
Type=Application
Categories=Development
GenericName=MyProgram
Keywords=ключевое_слово1; ключевое_слово2; ключевое_слово3
```

> Если ваше приложение должно открывать файлы, добавьте **%F** в конце строки Exec, после `myprogram`; если оно должно открывать URL, добавьте **%U**.

### 5) Добавление иконок hicolor (необязательно)

Иконки hicolor следуют следующей структуре папок.

[Эта запись в блоге](https://martin.hoppenheit.info/blog/2016/where-to-put-application-icons-on-linux/) советует помещать иконки в директории `hicolor` и `pixmaps`, согласно [документации по системе меню Debian](https://www.debian.org/doc/packaging-manuals/menu.html/ch3.html#s3.7) и [документации FreeDesktop](https://specifications.freedesktop.org/icon-theme-spec/icon-theme-spec-0.11.html#install_icons).

```
├── icons
│   └── hicolor
│       ├── 128x128
│       │   └── apps
│       │       └── myprogram.png
│       ├── 16x16
│       │   └── apps
│       │       └── myprogram.png
│       ├── 256x256
│       │   └── apps
│       │       └── myprogram.png
│       ├── 32x32
│       │   └── apps
│       │       └── myprogram.png
│       ├── 48x48
│       │   └── apps
│       │       └── myprogram.png
│       ├── 512x512
│       │   └── apps
│       │       └── myprogram.png
│       ├── 64x64
│       │   └── apps
│       │       └── myprogram.png
│       └── scalable
│           └── apps
│               └── myprogram.svg
```

### 6) Компиляция пакета `.deb`

```
# для архитектуры x64 рекомендуется суффикс amd64.
dpkg-deb --root

-owner-group --build ./staging_folder/ "./myprogram_${versionName}_amd64.deb"
```

## Пример скрипта оболочки Linux для всего процесса

```bash
#!/bin/bash

# Очистка
rm -rf ./out/
rm -rf ./staging_folder/

# Публикация .NET
# рекомендуется self-contained, чтобы конечным пользователям не нужно было устанавливать .NET
dotnet publish "./src/MyProgram.Desktop/MyProgram.Desktop.csproj" \
  --verbosity quiet \
  --nologo \
  --configuration Release \
  --self-contained true \
  --runtime linux-x64 \
  --output "./out/linux-x64"

# Папка подготовки
mkdir staging_folder

# Файл управления Debian
mkdir ./staging_folder/DEBIAN
cp ./src/MyProgram.Desktop.Debian/control ./staging_folder/DEBIAN

# Стартовый скрипт
mkdir ./staging_folder/usr
mkdir ./staging_folder/usr/bin
cp ./src/MyProgram.Desktop.Debian/myprogram.sh ./staging_folder/usr/bin/myprogram
chmod +x ./staging_folder/usr/bin/myprogram # установка разрешений на выполнение стартового скрипта

# Другие файлы
mkdir ./staging_folder/usr/lib
mkdir ./staging_folder/usr/lib/myprogram
cp -f -a ./out/linux-x64/. ./staging_folder/usr/lib/myprogram/ # копирование всех файлов из каталога публикации
chmod -R a+rX ./staging_folder/usr/lib/myprogram/ # установка разрешений на чтение всех файлов
chmod +x ./staging_folder/usr/lib/myprogram/myprogram_executable # установка разрешений на выполнение основного исполняемого файла

# Ярлык на рабочем столе
mkdir ./staging_folder/usr/share
mkdir ./staging_folder/usr/share/applications
cp ./src/MyProgram.Desktop.Debian/MyProgram.desktop ./staging_folder/usr/share/applications/MyProgram.desktop

# Иконка на рабочем столе
# 1024px x 1024px PNG, как у VS Code для его иконки
mkdir ./staging_folder/usr/share/pixmaps
cp ./src/MyProgram.Desktop.Debian/myprogram_icon_1024px.png ./staging_folder/usr/share/pixmaps/myprogram.png

# Иконки hicolor
mkdir ./staging_folder/usr/share/icons
mkdir ./staging_folder/usr/share/icons/hicolor
mkdir ./staging_folder/usr/share/icons/hicolor/scalable
mkdir ./staging_folder/usr/share/icons/hicolor/scalable/apps
cp ./misc/myprogram_logo.svg ./staging_folder/usr/share/icons/hicolor/scalable/apps/myprogram.svg

# Создание файла .deb
dpkg-deb --root-owner-group --build ./staging_folder/ ./myprogram_3.1.0_amd64.deb
```

## Установка

```Bash
sudo apt install ./myprogram_3.1.0_amd64.deb
```

## Удаление

```Bash
sudo apt remove myprogram
```

## Сторонние инструменты публикации для Debian / Ubuntu

* [dotnet-packaging](https://github.com/quamotion/dotnet-packaging)
* [DotnetPackaging](https://github.com/SuperJMN/DotnetPackaging)
* [PupNet-Deploy](https://github.com/kuiperzone/PupNet-Deploy)