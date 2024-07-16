# Редактирование стилей компонентов

Файл `src/Gml.Launcher/Assets/Styles/Classes.axaml` содержит определения стилей, используемых в проекте Gml.Launcher.
Эти стили применяются к различным элементам интерфейса для обеспечения единообразного внешнего вида и поведения. В этом
файле используются как `Style`, так и `Animation`, что позволяет гибко управлять визуальными аспектами интерфейса.

## Комментарии по стилям
```xml
<Styles xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:components="clr-namespace:Gml.Launcher.Views.Components">

    <!-- Предпросмотр дизайна -->
    <Design.PreviewWith>
        <components:StackFrameBorder Padding="20">
            <TextBox Text="Поле для ввода"/>
        </components:StackFrameBorder>
    </Design.PreviewWith>

    <!-- Стиль для всех TextBlock -->
    <Style Selector="TextBlock">
        <Setter Property="FontFamily" Value="Manrope"/>
    </Style>

    <!-- Стиль для TextBlock с классом FormText -->
    <Style Selector="TextBlock.FormText">
        <Setter Property="Foreground" Value="{DynamicResource ContentColor}"/> <!-- Цвет текста -->
        <Setter Property="FontWeight" Value="SemiBold"/> <!-- Жирность текста -->
        <Setter Property="Margin" Value="0, 0, 0, 8"/> <!-- Отступы -->
    </Style>

    <!-- Стиль для CheckBox с классом FormCheckBox -->
    <Style Selector="CheckBox.FormCheckBox">
        <Setter Property="Foreground" Value="{DynamicResource ContentColor}"/> <!-- Цвет текста -->
        <Setter Property="FontWeight" Value="SemiBold"/> <!-- Жирность текста -->
        <Setter Property="Cursor" Value="Hand"/> <!-- Вид курсора -->
    </Style>

    <!-- Стиль для уменьшенной кнопки ползунка (Slider) с классом FormSlider -->
    <Style Selector="Slider.FormSlider /template/ RepeatButton#PART_DecreaseButton /template/ Border#TrackBackground">
        <Setter Property="Background" Value="{DynamicResource PrimaryColor}"/> <!-- Фоновый цвет -->
    </Style>

    <!-- Стиль для движка ползунка (Slider) с классом FormSlider -->
    <Style Selector="Slider.FormSlider /template/ Thumb#thumb /template/ Border">
        <Setter Property="Background" Value="{DynamicResource PrimaryColor}"/> <!-- Фоновый цвет -->
    </Style>

    <!-- Стиль для выбранного состояния CheckBox с классом FormCheckBox -->
    <Style Selector="CheckBox.FormCheckBox:checked /template/ Border#NormalRectangle">
        <Setter Property="Background" Value="{DynamicResource PrimaryColor}"/> <!-- Фоновый цвет -->
        <Setter Property="BorderBrush" Value="{DynamicResource PrimaryColor}"/> <!-- Цвет границы -->
    </Style>

    <!-- Стиль для TextBlock с классом HeadlineText -->
    <Style Selector="TextBlock.HeadlineText">
        <Setter Property="Margin" Value="0, 0, 0, 8"/> <!-- Отступы -->
        <Setter Property="Foreground" Value="{DynamicResource HeadlineColor}"/> <!-- Цвет текста -->
        <Setter Property="FontWeight" Value="Bold"/> <!-- Жирность текста -->
        <Setter Property="FontSize" Value="22"/> <!-- Размер текста -->
    </Style>

    <!-- Стиль для TextBox с классом FormBox -->
    <Style Selector="TextBox.FormBox">
        <Setter Property="Foreground" Value="{DynamicResource ContentColor}"/> <!-- Цвет текста -->
        <Setter Property="FontWeight" Value="SemiBold"/> <!-- Жирность текста -->
        <Setter Property="Background" Value="{DynamicResource FormBackgroundColor}"/> <!-- Фоновый цвет -->
        <Setter Property="BorderBrush" Value="{DynamicResource FormBorderColor}"/> <!-- Цвет границы -->
        <Setter Property="BorderThickness" Value="1"/> <!-- Толщина границы -->
        <Setter Property="CornerRadius" Value="10"/> <!-- Радиус закругления -->
        <Setter Property="Padding" Value="15"/> <!-- Внутренние отступы -->
    </Style>

    <!-- Стиль для TextBox с классом FormBox при наведении -->
    <Style Selector="TextBox.FormBox:pointerover /template/  Border#PART_BorderElement">
        <Setter Property="BorderBrush" Value="{DynamicResource FormBorderHoverColor}"/> <!-- Цвет границы при наведении -->
    </Style>

    <!-- Стиль для TextBox с классом FormBox при фокусе -->
    <Style Selector="TextBox.FormBox:focus /template/  Border#PART_BorderElement">
        <Setter Property="BorderBrush" Value="{DynamicResource PrimaryColor}"/> <!-- Цвет границы при фокусе -->
        <Setter Property="BorderThickness" Value="1"/> <!-- Толщина границы -->
        <Setter Property="Background" Value="{DynamicResource PrimaryTransparentColor}"/> <!-- Фоновый цвет -->
    </Style>

    <!-- Стиль для StackPanel с классом MarginBottom -->
    <Style Selector="StackPanel.MarginBottom">
        <Setter Property="Margin" Value="0, 0, 0, 15"/> <!-- Нижний отступ -->
    </Style>

    <!-- Стиль для Border с классом Separator -->
    <Style Selector="Border.Separator">
        <Setter Property="Margin" Value="0, 20"/> <!-- Отступы -->
        <Setter Property="Height" Value="1"/> <!-- Высота -->
        <Setter Property="Background" Value="{DynamicResource FormBorderColor}"/> <!-- Фоновый цвет -->
    </Style>

    <!-- Стиль для ProgressBar с классом ProgressBar -->
    <Style Selector="ProgressBar.ProgressBar">
        <Setter Property="Background" Value="{DynamicResource FormBorderColor}"/> <!-- Фоновый цвет -->
        <Setter Property="Foreground" Value="{DynamicResource PrimaryColor}"/> <!-- Цвет прогресса -->
    </Style>

    <!-- Стиль для ComboBox с классом ComboBoxStyle -->
    <Style Selector="ComboBox.ComboBoxStyle">
        <Setter Property="Background" Value="{DynamicResource FrameBackgroundColor}"/> <!-- Фоновый цвет -->
        <Setter Property="BorderBrush" Value="{DynamicResource FrameBackgroundBorderColor}"/> <!-- Цвет границы -->
        <Setter Property="Foreground" Value="{DynamicResource ContentColor}"/> <!-- Цвет текста -->
        <Setter Property="BorderThickness" Value="1"/> <!-- Толщина границы -->
        <Setter Property="CornerRadius" Value="20"/> <!-- Радиус закругления -->
        <Setter Property="Padding" Value="20, 15, 5, 15"/> <!-- Внутренние отступы -->
        <Setter Property="FontWeight" Value="Medium"/> <!-- Жирность текста -->
    </Style>

    <!-- Стиль для ComboBox с классом ComboBoxStyle при наведении -->
    <Style Selector="ComboBox.ComboBoxStyle:pointerover /template/ ContentPresenter">
        <Setter Property="BorderBrush" Value="{DynamicResource FrameBackgroundBorderColor}"/> <!-- Цвет границы при наведении -->
    </Style>

    <!-- Анимация для компонента ServerInfo с классом Animated -->
    <Style Selector="components|ServerInfo.Animated">
        <Style.Animations>
            <Animation Duration="0:0:0.5" IterationCount="1"> <!-- Длительность анимации 0.5 секунды -->
                <KeyFrame Cue="0%">
                    <Setter Property="Opacity" Value="0"/> <!-- Начало анимации: прозрачность 0 -->
                </KeyFrame>
                <KeyFrame Cue="100%">
                    <Setter Property="Opacity" Value="1"/> <!-- Конец анимации: прозрачность 1 -->
                </KeyFrame>
            </Animation>
        </Style.Animations>
    </Style>

</Styles>
```