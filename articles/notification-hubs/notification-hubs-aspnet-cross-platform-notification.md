---
title: Kullanıcılara Azure Notification hubs'ı (ASP.NET) ile platformlar arası bildirim gönderme
description: Tek bir istekte, tüm platformları hedefleyen bir platformu belirsiz bildirim göndermek için Notification hubs'ı şablonları kullanmayı öğrenin.
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 11d2131b-f683-47fd-a691-4cdfc696f62b
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: multiple
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 0f92b49c9d77029a9624782b49eb23f7083c49aa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60872264"
---
# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Notification Hubs ile kullanıcılar için platformlar arası bildirim gönderme

Önceki bir öğreticide [Bildirim hub'larıyla kullanıcılara bildirim gönderme], belirli bir kimliği doğrulanmış kullanıcı için kaydedilen tüm cihazlara anında iletme bildirimleri öğrendiniz. Bu öğreticide, her desteklenen istemci platformu için bildirim göndermek için birden çok istek gerekirdi. Azure Notification Hubs ile belirli bir cihaza nasıl bildirimleri almak istediğini belirtebilirsiniz şablonları destekler. Bu yöntem, platformlar arası bildirim gönderme basitleştirir.

Bu makalede, tek bir istekte, tüm platformları hedefleyen bir platformu belirsiz bildirim göndermek için şablonları yararlanmak nasıl gösterir. Şablonlar hakkında daha ayrıntılı bilgi için bkz: [Azure Notification Hubs'a genel bakış][Templates].

> [!IMPORTANT]
> Windows Phone projeleri 8.1 ve önceki Visual Studio 2017'de desteklenmez. Daha fazla bilgi için bkz. [Visual Studio 2017 Platform Desteği ve Uyumluluk](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

> [!NOTE]
> Notification Hubs ile birden çok şablonun aynı etiketi taşıyan bir cihaz kaydedebilir. Bu durumda, cihaz her şablon için bir etiket sonuçları birden çok bildirim hedefleyen gelen iletiyi teslim. Bu işlem, hem bir rozet ve Windows Store uygulamasında bir bildirim olarak gibi birden çok görsel bildirimler aynı iletiyi görüntülemek sağlar.

## <a name="send-cross-platform-notifications-using-templates"></a>Şablonları kullanarak platformlar arası bildirimler gönderme

Şablonları kullanarak çapraz platform bildirimleri göndermek için aşağıdakileri yapın:

1. Visual Studio Çözüm Gezgini'nde **denetleyicileri** klasörünü ve sonra RegisterController.cs dosyasını açın.

2. Kod bloğunu bulun `Put` yeni bir kayıt oluşturur ve sonra değiştirmek yöntemi `switch` aşağıdaki kod ile içerik:

    ```csharp
    switch (deviceUpdate.Platform)
    {
        case "mpns":
            var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                    "<wp:Toast>" +
                        "<wp:Text1>$(message)</wp:Text1>" +
                    "</wp:Toast> " +
                "</wp:Notification>";
            registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
            break;
        case "wns":
            toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
            registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
            break;
        case "apns":
            var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
            registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
            break;
        case "fcm":
            var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
            registration = new FcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
            break;
        default:
            throw new HttpResponseException(HttpStatusCode.BadRequest);
    }
    ```

    Bu kod, bir şablon kayıt yerine yerel bir kayıt oluşturmak için platforma özgü yöntemini çağırır. Şablon kayıtları yerel kayıtları türetmek için var olan kayıtları değiştirmek gerekmez.

3. İçinde `Notifications` denetleyicisi değiştirin `sendNotification` yöntemini aşağıdaki kod ile:

    ```csharp
    public async Task<HttpResponseMessage> Post()
    {
        var user = HttpContext.Current.User.Identity.Name;
        var userTag = "username:" + user;

        var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
        await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

        return Request.CreateResponse(HttpStatusCode.OK);
    }
    ```

    Yerel bir yükü belirtmek zorunda olmadan bu kodu aynı zamanda, tüm platformlar için bir bildirim gönderir. Bildirim hub'ları oluşturur ve sağlanan her cihaza doğru yükü sunar *etiketi* kayıtlı şablonları belirtildiği gibi bir değer.

4. Webapı arka uç projeniz yeniden yayımlayın.

5. İstemci uygulamayı yeniden çalıştırın ve ardından kaydın başarılı olduğunu doğrulayın.

6. (İsteğe bağlı) İkinci bir cihaz için istemci uygulaması dağıtın ve ardından uygulamayı çalıştırın.
    Her cihaza bir bildirim görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticiyi tamamladığınıza göre bildirim hub'ları ve bu konularda şablonları hakkında daha fazla bilgi bulabilirsiniz:

* [Use Notification Hubs to send breaking news]: Demonstrates another scenario for using templates.
* [Azure Notification Hubs'a genel bakış][Templates]: Şablonları hakkında daha ayrıntılı bilgi içerir.

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[Push to users ASP.NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Push to users Mobile Services]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Visual Studio 2012 Express for Windows 8]: https://go.microsoft.com/fwlink/?LinkId=257546

[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: https://go.microsoft.com/fwlink/p/?LinkId=314257
[Bildirim hub'larıyla kullanıcılara bildirim gönderme]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: https://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: https://msdn.microsoft.com/library/windowsazure/jj927172.aspx
