# Сборка из иcходников GitHub

### Загрузка лаунчера

Выполните папку в удобной для вас директории

<tabs>
    <tab title="Стабильная версия">
      <code-block lang="bash">
        git clone --recursive https://github.com/GamerVII-NET/Gml.Launcher.git
      </code-block>
    </tab>
    <tab title="Последняя актуальная">
      <code-block lang="bash">
            git clone --recursive --branch dev https://github.com/GamerVII-NET/Gml.Launcher.git
      </code-block>
    </tab>
</tabs>

После установки у вас будет папка Gml.Launcher, откройте в ней `Gml.Launcher.sln` в
одном [из загруженных IDE](Installation.md)

## Настройка проекта

1. Откройте файл `src/Gml.Launcher/Assets/Resources/ResourceKeysDictionary.cs`
2. Отредактируйте файл

```C#
public static class ResourceKeysDictionary
{
    ...
    
    public const string Host = "{{HOST}}";
    public const string FolderName = "{{FOLDER_NAME}}";
}
```

> - Замените переменную `{{HOST}}` на адрес [вашего API](server-install-from-source.md)
> - Замените переменную `{{FOLDER_NAME}}` на наименование папки, которая будет создаваться у пользователей

3. Соберите проект

<tabs>
    <tab title="Visual studio">
        <img src="publish-visual-studio-1.png" alt="Установка Visual Studio 1" />
        <img src="publish-visual-studio-2.png" alt="Установка Visual Studio 2" />
        <img src="publish-visual-studio-3.png" alt="Установка Visual Studio 3" />
    </tab>
    <tab title="JetBrains Rider">
        <img src="publish-rider-1.png" alt="Установка Rider 1" />
        <img src="publish-rider-2.png" alt="Установка Rider 2" />

    </tab>
</tabs>