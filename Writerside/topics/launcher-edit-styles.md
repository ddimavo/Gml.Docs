# Редактирование цветов

Файл `src/Gml.Launcher/Assets/Styles/Colors.axaml` содержит определения цветовых ресурсов, используемых в проекте Gml.Launcher. Эти цвета определены с помощью `SolidColorBrush` и `LinearGradientBrush`, и они могут быть использованы в различных элементах интерфейса.

## SolidColorBrush

### Основные цвета
- **PrimaryColor** (#008C45): Основной цвет.
- **PrimaryColorHover** (#01763B): Основной цвет при наведении.
- **PrimaryTransparentColor** (#101412): Прозрачный основной цвет.

### Тексты и контент
- **HeadlineColor** (#FFFFFF): Цвет заголовков.
- **ContentColor** (#969696): Цвет контента.

### Фоновые цвета
- **BackgroundColor** (#0E0E0E): Основной фоновый цвет.
- **FrameBackgroundColor** (#111111): Фоновый цвет для рамок.
- **FrameBackgroundBorderColor** (#2E2E2E): Цвет границы для рамок.

### Второстепенные цвета
- **SecondaryColor** (#060B0D): Второстепенный цвет.
- **SecondaryColorHover** (#131719): Второстепенный цвет при наведении.

### Цвета форм
- **FormBackgroundColor** (#151515): Фоновый цвет формы.
- **FormBackgroundHoverColor** (#1C1C1C): Фоновый цвет формы при наведении.
- **FormBorderColor** (#212121): Цвет границы формы.
- **FormBorderHoverColor** (#2C2C2C): Цвет границы формы при наведении.

### Прочие цвета
- **BadgeBorder** (#20FFFFFF): Цвет границы значка.

## LinearGradientBrush

### Градиенты профиля
- **ProfileLinearGradient**: Линейный градиент для профиля.
    - **StartPoint**: 50%, -80%
    - **EndPoint**: 50%, 100%
    - **Opacity**: 0.3
    - **GradientStops**:
        - Offset="0", Color="Transparent"
        - Offset="0.1", Color="#10018A44"
        - Offset="1", Color="#018A44"

- **ProfileImageLinearGradient**: Линейный градиент для изображения профиля.
    - **StartPoint**: 50%, -80%
    - **EndPoint**: 50%, 100%
    - **Opacity**: 0.2
    - **GradientStops**:
        - Offset="0", Color="#111111"
        - Offset="1", Color="#111111"

### Фоновые градиенты
- **BackgroundOverlay**: Линейный градиент для фона.
    - **StartPoint**: 50%, -80%
    - **EndPoint**: 50%, 100%
    - **Opacity**: 0.9
    - **GradientStops**:
        - Offset="0", Color="Transparent"
        - Offset="0.3", Color="#0E0E0E"
        - Offset="1", Color="#0E0E0E"


### Пример настроенного файла

```xml
<ResourceDictionary xmlns="https://github.com/avaloniaui"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <!-- Основные цвета -->
    <SolidColorBrush x:Key="PrimaryColor">#008C45</SolidColorBrush> <!-- Основной цвет -->
    <SolidColorBrush x:Key="PrimaryColorHover">#01763B</SolidColorBrush> <!-- Основной цвет при наведении -->
    <SolidColorBrush x:Key="PrimaryTransparentColor">#101412</SolidColorBrush> <!-- Прозрачный основной цвет -->

    <!-- Цвета текста и контента -->
    <SolidColorBrush x:Key="HeadlineColor">#FFFFFF</SolidColorBrush> <!-- Цвет заголовков -->
    <SolidColorBrush x:Key="ContentColor">#969696</SolidColorBrush> <!-- Цвет контента -->

    <!-- Фоновые цвета -->
    <SolidColorBrush x:Key="BackgroundColor">#0E0E0E</SolidColorBrush> <!-- Основной фоновый цвет -->
    <SolidColorBrush x:Key="FrameBackgroundColor">#111111</SolidColorBrush> <!-- Фоновый цвет для рамок -->
    <SolidColorBrush x:Key="FrameBackgroundBorderColor">#2E2E2E</SolidColorBrush> <!-- Цвет границы для рамок -->

    <!-- Второстепенные цвета -->
    <SolidColorBrush x:Key="SecondaryColor">#060B0D</SolidColorBrush> <!-- Второстепенный цвет -->
    <SolidColorBrush x:Key="SecondaryColorHover">#131719</SolidColorBrush> <!-- Второстепенный цвет при наведении -->

    <!-- Цвета форм -->
    <SolidColorBrush x:Key="FormBackgroundColor">#151515</SolidColorBrush> <!-- Фоновый цвет формы -->
    <SolidColorBrush x:Key="FormBackgroundHoverColor">#1C1C1C</SolidColorBrush> <!-- Фоновый цвет формы при наведении -->
    <SolidColorBrush x:Key="FormBorderColor">#212121</SolidColorBrush> <!-- Цвет границы формы -->
    <SolidColorBrush x:Key="FormBorderHoverColor">#2C2C2C</SolidColorBrush> <!-- Цвет границы формы при наведении -->

    <!-- Прочие цвета -->
    <SolidColorBrush x:Key="BadgeBorder">#20FFFFFF</SolidColorBrush> <!-- Цвет границы значка -->

    <!-- Градиенты профиля -->
    <LinearGradientBrush x:Key="ProfileLinearGradient" StartPoint="50%, -80%" EndPoint="50%, 100%" Opacity=".3">
        <LinearGradientBrush.GradientStops>
            <GradientStop Offset="0" Color="Transparent" /> <!-- Начало градиента: прозрачный -->
            <GradientStop Offset="0.1" Color="#10018A44" /> <!-- Промежуточный цвет градиента -->
            <GradientStop Offset="1" Color="#018A44" /> <!-- Конец градиента: основной цвет -->
        </LinearGradientBrush.GradientStops>
    </LinearGradientBrush>

    <LinearGradientBrush x:Key="ProfileImageLinearGradient" StartPoint="50%, -80%" EndPoint="50%, 100%" Opacity=".2">
        <LinearGradientBrush.GradientStops>
            <GradientStop Offset="0" Color="#111111" /> <!-- Начало градиента: темный цвет -->
            <GradientStop Offset="1" Color="#111111" /> <!-- Конец градиента: темный цвет -->
        </LinearGradientBrush.GradientStops>
    </LinearGradientBrush>

    <!-- Фоновые градиенты -->
    <LinearGradientBrush x:Key="BackgroundOverlay" StartPoint="50%, -80%" EndPoint="50%, 100%" Opacity=".9">
        <LinearGradientBrush.GradientStops>
            <GradientStop Offset="0" Color="Transparent" /> <!-- Начало градиента: прозрачный -->
            <GradientStop Offset="0.3" Color="#0E0E0E" /> <!-- Промежуточный цвет градиента -->
            <GradientStop Offset="1" Color="#0E0E0E" /> <!-- Конец градиента: основной фоновый цвет -->
        </LinearGradientBrush.GradientStops>
    </LinearGradientBrush>

</ResourceDictionary>
```