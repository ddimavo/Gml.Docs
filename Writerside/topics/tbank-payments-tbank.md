# Платежи через мобильное приложение

Данная страница предоставляет информацию по проведению платежа исключительно для ТБанк. Здесь вы найдёте все необходимые
инструкции и рекомендации для пользователей приложения ТБанк.

## Предварительные требования

> Перед тем как провести платеж, его нужно сформировать. Этот процесс обязателен для успешного проведения платежа.
> Подробности можно найти в [информации](tbank-payments-form.md#tbank-payment-init).

## Пошаговая инструкция

### 1. Получение информации о платеже

Для получения информации о платеже используйте метод `GetPaymentInfoAsync(string paymentId)`. Этот метод асинхронно
возвращает кортеж, содержащий URL для перенаправления и QR-код в виде строки.

```C#
var qrCodeInfo = await _acquiringSdk.Tpay.GetPaymentInfoAsync(paymentId);
```

#### Описание возвращаемых значений:

- `RedirectUrl` — URL для перенаправления, куда нужно перейти для завершения платежа.
- `WebQR` — QR-код в виде строки, который можно отобразить для сканирования.

#### Пример использования:

```C#
var qrCodeInfo = await _acquiringSdk.Tpay.GetPaymentInfoAsync(paymentId);
Console.WriteLine($"Redirect URL: {qrCodeInfo.RedirectUrl}");
Console.WriteLine($"Web QR Code: {qrCodeInfo.WebQR}");
```

### 2. Получение QR-кода для платежа

Для получения QR-кода используйте метод `GetQRCodeAsync(string paymentId)`. Этот метод асинхронно возвращает поток (
`Stream`), содержащий изображение QR-кода.

```C#
var qrCode = await _acquiringSdk.Tpay.GetQRCodeAsync(paymentId);
```

#### Пример использования:

```C#
var qrCode = await _acquiringSdk.Tpay.GetQRCodeAsync(paymentId);
using (var fileStream = new FileStream("qr_code.png", FileMode.Create, FileAccess.Write))
{
    await qrCode.CopyToAsync(fileStream);
}
```

### 3. Полный пример

Ниже приведён полный пример кода, показывающий, как получить и обработать информацию о платеже и QR-код.

```C#
async Task ProcessPayment(string paymentId)
{
    // Получение информации о платеже
    var qrCodeInfo = await _acquiringSdk.Tpay.GetPaymentInfoAsync(paymentId);
    Console.WriteLine($"Redirect URL: {qrCodeInfo.RedirectUrl}");
    Console.WriteLine($"Web QR Code: {qrCodeInfo.WebQR}");
    
    // Получение QR-кода для платежа
    var qrCode = await _acquiringSdk.Tpay.GetQRCodeAsync(paymentId);
    using (var fileStream = new FileStream("qr_code.png", FileMode.Create, FileAccess.Write))
    {
        await qrCode.CopyToAsync(fileStream);
    }
}
```

С помощью этих методов и примеров кода вы можете быстро получить необходимую информацию о платеже и QR-код через SDK
TPay.