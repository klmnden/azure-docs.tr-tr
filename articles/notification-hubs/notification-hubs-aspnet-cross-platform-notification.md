---
title: Kullanıcılara Azure bildirim hub'ları (ASP.NET) ile platformlar arası bildirimleri gönderin
description: Bildirim hub'ları şablonları, tek bir istekte, tüm platformlar hedefleyen bir platform belirsiz bildirim göndermek için nasıl kullanılacağını öğrenin.
services: notification-hubs
documentationcenter: ''
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 11d2131b-f683-47fd-a691-4cdfc696f62b
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: multiple
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 95793aac3c25563e3af39f3c47cebdd06e25e35f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33777962"
---
# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Kullanıcılara Notification Hubs ile platformlar arası bildirimleri gönderin
Bir önceki öğreticideki [Notification Hubs kullanıcılara bildirme], belirli bir kimliği doğrulanmış kullanıcı için kaydedilen tüm cihazlara anında iletme bildirimleri öğrendiniz. Bu öğreticide, birden çok istek her desteklenen istemci platformu için bir bildirim gönderecek gerekirdi. Azure bildirim hub'ları ile belirli bir aygıt nasıl bildirimlerini almak istediği belirtebileceğiniz şablonları destekler. Bu yöntem, platformlar arası bildirimleri gönderme basitleştirir. 

Bu makalede, tek bir istekte, tüm platformlar hedefleyen bir platform belirsiz bildirim göndermek için şablonlar yararlanın gösterilmiştir. Şablonlar hakkında daha ayrıntılı bilgi için bkz: [Azure Notification Hubs'a genel bakış][Templates].

> [!IMPORTANT]
> Windows Phone projeleri 8.1 ve önceki Visual Studio 2017 içinde desteklenmez. Daha fazla bilgi için bkz. [Visual Studio 2017 Platform Desteği ve Uyumluluk](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

> [!NOTE]
> Bildirim hub'larıyla bir aygıt aynı etiketi ile birden fazla şablon kaydedebilirsiniz. Bu durumda, cihaz her şablon için bir tane birden fazla bildirim etiketi sonuçlarında hedefleyen gelen ileti teslim. Bu işlem, hem bir gösterge ve bir Windows mağazası uygulamasında bildirim olarak gibi birden çok görsel bildirimler aynı iletiyi görüntülemek sağlar.
> 
> 

Şablonları kullanarak platformlar arası bildirimleri göndermek için aşağıdakileri yapın:

1. Visual Studio'da Çözüm Gezgini'nde genişletin **denetleyicileri** klasörü ve sonra RegisterController.cs dosyasını açın.

2. Kod bloğunu bulun **Put** yeni bir kayıt oluşturur ve sonra değiştirmek yöntemi `switch` aşağıdaki kod ile içerik:
   
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
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }
   
    Bu kod, yerel bir kaydı yerine bir şablon kaydı oluşturmak için platforma özgü yöntemini çağırır. Şablon kayıtlar yerel kayıtları türetilen olduğundan, var olan kayıtlar değişiklik gerekmez.

3. İçinde **bildirimleri** denetleyicisi Değiştir **sendNotification** aşağıdaki kod ile yöntemi:
   
        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;
   
            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
    Yerel bir yükü belirtmek zorunda olmadan bu kodu aynı anda tüm platformlar için bir bildirim gönderir. Hub oluşturur ve sağlanan ile her aygıt için doğru yükü teslim bildirim *etiketi* kayıtlı şablonları belirtildiği gibi bir değer.

4. Webapı arka uç projeniz yeniden yayımlayın.

5. İstemci uygulamasını yeniden çalıştırın ve kaydın başarılı olduğunu doğrulayın.

6. (İsteğe bağlı) İkinci bir cihaz istemci uygulamasını dağıtın ve ardından uygulamayı çalıştırın.
    Her cihazda bir bildirim görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticiyi tamamladığınıza göre bildirim hub'ları ve bu konularda şablonları hakkında daha fazla bilgi:

* [Use Notification Hubs to send breaking news]: Demonstrates another scenario for using templates.
* [Azure Notification Hubs'a genel bakış][Templates]: şablonları hakkında daha ayrıntılı bilgi içerir.

<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Notification Hubs kullanıcılara bildirme]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
