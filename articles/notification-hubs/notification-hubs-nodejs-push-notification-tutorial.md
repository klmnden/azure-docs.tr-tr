---
title: Azure Notification Hubs ve Node.js ile anında iletme bildirimleri gönderme
description: Bir Node.js uygulamasından anında iletme bildirimleri göndermek için Notification hubs'ı kullanmayı öğrenin.
keywords: anında iletme bildirimi, anında iletme notifications,node.js anında iletme, ios anında iletme
services: notification-hubs
documentationcenter: nodejs
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: ded4749c-6c39-4ff8-b2cf-1927b3e92f93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 129127a2a43cd9a86e0a1e1cf538358b62381257
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706222"
---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Azure Notification Hubs ve Node.js ile anında iletme bildirimleri gönderme

[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a>Genel Bakış

> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika ücretsiz bir deneme hesabı oluşturma [Azure ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).

Bu kılavuz doğrudan Azure Notification Hubs'ın yardımıyla anında iletme bildirimleri göndermek nasıl gösterir bir [Node.js](https://nodejs.org) uygulama.

Aşağıdaki platformlarda uygulamalarına anında iletme bildirimleri gönderme senaryoları ele alınmaktadır:

- Android
- iOS
- Evrensel Windows Platformu
- Windows Phone

## <a name="notification-hubs"></a>Notification Hubs

Azure Notification Hubs, mobil cihazlara anında iletme bildirimleri göndermek için kullanımı kolay, çok platformlu, ölçeklenebilir bir altyapı sağlar. Hizmet altyapısı hakkında daha fazla bilgi için bkz: [Azure Notification hubs'ı](https://msdn.microsoft.com/library/windowsazure/jj927170.aspx) sayfası.

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma

Bu öğreticide ilk adım, yeni ve boş bir Node.js uygulaması oluşturmaktır. Bir Node.js uygulaması oluşturma ile ilgili yönergeler için bkz: [oluşturma ve bir Node.js uygulaması için Azure Web sitesine dağıtma][nodejswebsite] , [Node.js Cloud Service][Node.js Cloud Service] Windows PowerShell kullanarak veya [WebMatrix ile Web sitesi] [webmatrix].

## <a name="configure-your-application-to-use-notification-hubs"></a>Uygulamanızı bildirim hub'ları kullanacak şekilde yapılandırma

Azure Notification hubs'ı kullanmak için indirme ve Node.js kullanma için ihtiyaç duyduğunuz [azure paketi](https://www.npmjs.com/package/azure), yerleşik bir anında iletme bildirimi REST Hizmetleri ile iletişim kurmasına yardımcı kitaplıklar kümesi içerir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Bir paketi almasını düğüm paketi Yöneticisi'ni (NPM) kullanın

1. Bir komut satırı arabirimi gibi kullanın **PowerShell** (Windows) **Terminal** (Mac) veya **Bash** (Linux) ve boş uygulamanızı oluşturduğunuz klasöre gidin.
2. Yürütme `npm install azure-sb` komut penceresinde.
3. El ile çalıştırabileceğiniz `ls` veya `dir` doğrulamak için komutu bir `node_modules` klasörü oluşturuldu.
4. Bu klasörün içinde Bul **azure** bildirim hub'ı erişmek için ihtiyacınız olan kitaplıkları içeren paket.

> [!NOTE]
> Resmi üzerinde NPM yükleme hakkında daha fazla bilgi [NPM blog](https://blog.npmjs.org/post/85484771375/how-to-install-npm).

### <a name="import-the-module"></a>Modülü içeri aktarın
Bir metin düzenleyicisi kullanarak, üst kısmına aşağıdakileri ekleyin `server.js` uygulamanın dosya:

```javascript
var azure = require('azure-sb');
```

### <a name="set-up-an-azure-notification-hub-connection"></a>Bir Azure bildirim hub'ı bağlantısı kurma

`NotificationHubService` Nesne, notification hubs ile çalışmanıza olanak tanır. Aşağıdaki kod oluşturur bir `NotificationHubService` adlı bildirim hub'ı için nesne `hubname`. En ekleme `server.js` dosya, azure modülü içeri aktarmak için deyim sonra:

```javascript
var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');
```

Bağlantı elde `connectionstring` değerini [Azure Portal] aşağıdaki adımları gerçekleştirerek:

1. Sol gezinti bölmesinden **Gözat**.
2. Seçin **Notification hubs'ı**ve ardından örnek için kullanmak istediğiniz hub'ı bulun. Başvurabilirsiniz [Windows Store Başlarken Öğreticisi](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) yeni bir bildirim hub'ı oluşturma konusunda yardıma ihtiyacınız varsa.
3. Seçin **ayarları**.
4. Tıklayarak **erişim ilkeleri**. Hem paylaşılan ve tam erişim bağlantı dizelerini görürsünüz.

![Azure portalı - bildirim hub'ları](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> Kullanarak bağlantı dizenizi alabilirsiniz **Get-AzureSbNamespace** cmdlet'i tarafından sağlanan [Azure PowerShell](/powershell/azureps-cmdlets-docs) veya **azure sb ad alanını göster** komutunu [Azure komut satırı arabirimi (Azure CLI)](../cli-install-nodejs.md).

## <a name="general-architecture"></a>Genel mimari

`NotificationHubService` Nesnesi belirli cihazlarınız ve uygulamalarınız için anında iletme bildirimleri göndermek için şu nesne örnekleri kullanıma sunar:

- **Android** -kullanın `GcmService` kullanılabilir nesnesi `notificationHubService.gcm`
- **iOS** -kullanın `ApnsService` adresinden erişilebilen bir nesne `notificationHubService.apns`
- **Windows Phone** -kullanın `MpnsService` kullanılabilir nesnesi `notificationHubService.mpns`
- **Evrensel Windows platformu** -kullanın `WnsService` kullanılabilir nesnesi `notificationHubService.wns`

### <a name="how-to-send-push-notifications-to-android-applications"></a>Nasıl yapılır: Android uygulamalarına anında iletme bildirimleri gönderme

`GcmService` Nesnesi sağlayan bir `send` Android uygulamalarına anında iletme bildirimleri göndermek için kullanılan yöntem. `send` Yöntemi aşağıdaki parametreleri kabul eder:

- **Etiketleri** -etiket tanımlayıcısı. Hiçbir etiket sağlanırsa, tüm istemciler için bildirim gönderilir.
- **Yükü** -ileti JSON veya ham dize yükü.
- **Geri çağırma** -geri çağırma işlevi.

Yük biçimi hakkında daha fazla bilgi için bkz. [yükü belgelerine](https://distriqt.github.io/ANE-PushNotifications/m.FCM-GCM%20Payload).

Aşağıdaki kod `GcmService` örneği tarafından açığa çıkarılan `NotificationHubService` tüm kayıtlı istemcilere anında iletme bildirimi göndermek için.

```javascript
var payload = {
  data: {
    message: 'Hello!'
  }
};
notificationHubService.gcm.send(null, payload, function(error){
  if(!error){
    //notification sent
  }
});
```

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Nasıl yapılır: İOS uygulamaları için anında iletme bildirimleri gönderme

Yukarıda Android uygulamaları ile aynı `ApnsService` nesnesi sağlayan bir `send` iOS uygulamaları için anında iletme bildirimleri göndermek için kullanılan yöntem. `send` Yöntemi aşağıdaki parametreleri kabul eder:

- **Etiketleri** -etiket tanımlayıcısı. Hiçbir etiket sağlanırsa, tüm istemciler için bildirim gönderilir.
- **Yükü** -ileti JSON ya da dize yükü.
- **Geri çağırma** -geri çağırma işlevi.

Yük biçimi daha fazla bilgi için **bildirim yükü** bölümünü [yerel ve anında iletilen bildirim Programlama Kılavuzu](https://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) belge.

Aşağıdaki kod `ApnsService` örneği tarafından açığa çıkarılan `NotificationHubService` tüm istemcilere bir uyarı iletisi göndermek için:

```javascript
var payload={
    alert: 'Hello!'
  };
notificationHubService.apns.send(null, payload, function(error){
  if(!error){
      // notification sent
  }
});
```

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Nasıl yapılır: Windows Phone uygulamalarına anında iletme bildirimleri gönderme

`MpnsService` Nesnesi sağlayan bir `send` Windows Phone uygulamalarına anında iletme bildirimleri göndermek için kullanılan yöntem. `send` Yöntemi aşağıdaki parametreleri kabul eder:

- **Etiketleri** -etiket tanımlayıcısı. Hiçbir etiket sağlanırsa, tüm istemciler için bildirim gönderilir.
- **Yükü** -ileti XML yükü.
- **TargetName**  -  `toast` kutlama bildirimleri için. `token` kutucuk bildirimleri için.
- **NotificationClass** -bildirim önceliğini. Bkz: **HTTP üstbilgi öğelerini** bölümünü [anında iletme bildirimleri bir sunucudan](https://msdn.microsoft.com/library/hh221551.aspx) belge için geçerli değerler.
- **Seçenekleri** : isteğe bağlı istek üst.
- **Geri çağırma** -geri çağırma işlevi.

Geçerli bir listesi için `TargetName`, `NotificationClass` ve üst bilgi seçeneklerini kullanıma [anında iletme bildirimleri bir sunucudan](https://msdn.microsoft.com/library/hh221551.aspx) sayfası.

Aşağıdaki örnek kod kullandığı `MpnsService` örneği tarafından açığa çıkarılan `NotificationHubService` bir anında iletme bildirimi göndermek için:

```javascript
var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
  if(!error){
    //notification sent
  }
});
```

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Nasıl yapılır: Evrensel Windows Platformu (UWP) uygulamaları için anında iletme bildirimleri gönderme

`WnsService` Nesnesi sağlayan bir `send` Evrensel Windows platformu uygulamaları için anında iletme bildirimleri göndermek için kullanılan yöntem.  `send` Yöntemi aşağıdaki parametreleri kabul eder:

- **Etiketleri** -etiket tanımlayıcısı. Hiçbir etiket sağlanırsa, kayıtlı tüm istemcilere bildirim gönderilir.
- **Yükü** -XML ileti yükü.
- **Tür** -bildirim türü.
- **Seçenekleri** : isteğe bağlı istek üst.
- **Geri çağırma** -geri çağırma işlevi.

Geçerli türler ve istek üstbilgileri listesi için bkz: [anında iletme bildirimi hizmeti istek ve yanıt üstbilgileri](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

Aşağıdaki kod `WnsService` örneği tarafından açığa çıkarılan `NotificationHubService` bir UWP uygulamasına bir anında iletme bildirimi göndermek için:

```javascript
var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
  if(!error){
      // notification sent
  }
});
```

## <a name="next-steps"></a>Sonraki Adımlar

Yukarıdaki örnek kod, hizmet altyapısı, çok çeşitli cihazlara anında iletme bildirimleri göndermeyi kolayca oluşturmanızı sağlar. Node.js ile Notification hubs'ı kullanmanın temellerini öğrendiğinize göre bu özellikler daha fazla nasıl genişletebileceğiniz hakkında daha fazla bilgi için bu bağlantıları izleyin.

- MSDN işinize yarayacak [Azure Notification hubs'ı](https://msdn.microsoft.com/library/azure/jj927170.aspx).
- Ziyaret [Node için Azure SDK] GitHub deposunu daha fazla örnekleri ve uygulama ayrıntıları.

[Node için Azure SDK]: https://github.com/WindowsAzure/azure-sdk-for-node
[Next Steps]: #nextsteps
[What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
[Create a Service Namespace]: #create-a-service-namespace
[Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
[Create a Node.js Application]: #Create_a_Nodejs_Application
[Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
[How to: Create a Topic]: #How_to_Create_a_Topic
[How to: Create Subscriptions]: #How_to_Create_Subscriptions
[How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
[How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
[How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
[How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
[1]: #Next_Steps
[Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
[image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
[2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
[3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
[4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
[5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Azure Service Bus Notification Hubs]: https://msdn.microsoft.com/library/windowsazure/jj927170.aspx
[SqlFilter]: https://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
[Web Site with WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs
[webmatrix]: https://docs.microsoft.com/aspnet/web-pages/videos/introduction/create-a-website-using-webmatrix
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
[Azure Portal]: https://portal.azure.com
