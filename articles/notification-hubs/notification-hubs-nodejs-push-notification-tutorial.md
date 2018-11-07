---
title: Azure Notification Hubs ve Node.js ile anında iletme bildirimleri gönderme
description: Bir Node.js uygulamasından anında iletme bildirimleri göndermek için Notification hubs'ı kullanmayı öğrenin.
keywords: anında iletme bildirimi, anında iletme notifications,node.js anında iletme, ios anında iletme
services: notification-hubs
documentationcenter: nodejs
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: ded4749c-6c39-4ff8-b2cf-1927b3e92f93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 8e4c97a710cc9e6d3af4ebdd7dc97bda9f8d02ed
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228445"
---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Azure Notification Hubs ve Node.js ile anında iletme bildirimleri gönderme
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a>Genel Bakış
> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).
> 
> 

Bu kılavuz doğrudan bir Node.js uygulamasından Azure Notification Hubs'ın yardımıyla anında iletme bildirimleri göndermek nasıl gösterir. 

Aşağıdaki platformlarda uygulamalarına anında iletme bildirimleri gönderme senaryoları ele alınmaktadır:

* Android
* iOS
* Windows Phone
* Evrensel Windows Platformu 

Bildirim hub'ları hakkında daha fazla bilgi için bkz. [sonraki adımlar](#next) bölümü.

## <a name="what-are-notification-hubs"></a>Bildirim Hub'ları nedir?
Azure Notification Hubs, mobil cihazlara anında iletme bildirimleri göndermek için kullanımı kolay, çok platformlu, ölçeklenebilir bir altyapı sağlar. Hizmet altyapısı hakkında daha fazla bilgi için bkz: [Azure Notification hubs'ı](https://msdn.microsoft.com/library/windowsazure/jj927170.aspx) sayfası.

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma
Bu öğreticide ilk adım, yeni ve boş bir Node.js uygulaması oluşturmaktır. Bir Node.js uygulaması oluşturma ile ilgili yönergeler için bkz: [oluşturun ve bir Node.js uygulamasını Azure Web sitesini dağıtma][nodejswebsite], [Node.js bulut hizmeti] [ Node.js Cloud Service] Windows PowerShell kullanarak veya [WebMatrix ile Web sitesi][webmatrix].

## <a name="configure-your-application-to-use-notification-hubs"></a>Uygulamanızı bildirim hub'ları kullanacak şekilde yapılandırma
Azure Notification hubs'ı kullanmak için indirme ve Node.js kullanma için ihtiyaç duyduğunuz [azure paketi](https://www.npmjs.com/package/azure), yerleşik bir anında iletme bildirimi REST Hizmetleri ile iletişim kurmasına yardımcı kitaplıklar kümesi içerir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Bir paketi almasını düğüm paketi Yöneticisi'ni (NPM) kullanın
1. Bir komut satırı arabirimi gibi kullanın **PowerShell** (Windows) **Terminal** (Mac) veya **Bash** (Linux) ve boş uygulamanızı oluşturduğunuz klasöre gidin.
2. Tür **npm yükleme azure sb** komut penceresinde.
3. El ile çalıştırabilirsiniz **ls** veya **dir** doğrulamak için komutu bir **düğüm\_modülleri** klasörü oluşturuldu. Bu klasörün içinde Bul **azure** bildirim hub'ı erişmek için ihtiyacınız olan kitaplıkları içeren paket.

> [!NOTE]
> Resmi üzerinde NPM yükleme hakkında daha fazla bilgi [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 
> 
> 

### <a name="import-the-module"></a>Modülü içeri aktarın
Bir metin düzenleyicisi kullanarak, üst kısmına aşağıdakileri ekleyin **server.js** uygulamanın dosya:

    var azure = require('azure');

### <a name="set-up-an-azure-notification-hub-connection"></a>Bir Azure bildirim hub'ı bağlantısı kurma
**NotificationHubService** nesne, notification hubs ile çalışmanıza olanak tanır. Aşağıdaki kod oluşturur bir **NotificationHubService** adlı bildirim hub'ı için nesne **hubname**. En ekleme **server.js** dosya, azure modülü içeri aktarmak için deyim sonra:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

Bağlantı **connectionstring** değer elde edilebilir gelen [Azure Portal] aşağıdaki adımları gerçekleştirerek:

1. Sol gezinti bölmesinden **Gözat**.
2. Seçin **Notification hubs'ı**ve ardından örnek için kullanmak istediğiniz hub'ı bulun. Başvurabilirsiniz [Windows Store Başlarken Öğreticisi](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) yeni bir bildirim hub'ı oluşturmayla ilgili Yardım gerekiyorsa.
3. Seçin **ayarları**.
4. Tıklayarak **erişim ilkeleri**. Hem paylaşılan ve tam erişim bağlantı dizelerini görürsünüz.

![Azure portalı - bildirim hub'ları](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> Kullanarak bağlantı dizenizi alabilirsiniz **Get-AzureSbNamespace** cmdlet'i tarafından sağlanan [Azure PowerShell](/powershell/azureps-cmdlets-docs) veya **azure sb ad alanını göster** komutunu [Azure komut satırı arabirimi (Azure CLI)](../cli-install-nodejs.md).
> 
> 

## <a name="general-architecture"></a>Genel mimari
**NotificationHubService** nesnesi belirli cihazlarınız ve uygulamalarınız için anında iletme bildirimleri göndermek için şu nesne örnekleri kullanıma sunar:

* **Android** -kullanın **GcmService** kullanılabilir olan nesne **notificationHubService.gcm**
* **iOS** -kullanın **ApnsService** adresinden erişilebilen bir nesne **notificationHubService.apns**
* **Windows Phone** -kullanın **MpnsService** kullanılabilir olan nesne **notificationHubService.mpns**
* **Evrensel Windows platformu** -kullanın **WnsService** kullanılabilir olan nesne **notificationHubService.wns**

### <a name="how-to-send-push-notifications-to-android-applications"></a>Nasıl yapılır: Android uygulamalarına anında iletme bildirimleri gönderme
**GcmService** nesnesi sağlayan bir **Gönder** Android uygulamalarına anında iletme bildirimleri göndermek için kullanılan yöntem. **Gönder** yöntemi aşağıdaki parametreleri kabul eder:

* **Etiketleri** -etiket tanımlayıcısı. Hiçbir etiket sağlanırsa, tüm istemciler için bildirim gönderilir.
* **Yükü** -ileti JSON veya ham dize yükü.
* **Geri çağırma** -geri çağırma işlevi.

Yük biçimi hakkında daha fazla bilgi için bkz. **yükü** bölümünü [uygulama GCM Server](http://developer.android.com/google/gcm/server.html#payload) belge.

Aşağıdaki kod **GcmService** örneği tarafından açığa çıkarılan **NotificationHubService** tüm kayıtlı istemcilere anında iletme bildirimi göndermek için.

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

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Nasıl yapılır: iOS uygulamaları için anında iletme bildirimleri gönderme
Yukarıda Android uygulamaları ile aynı **ApnsService** nesnesi sağlar bir **Gönder** iOS uygulamaları için anında iletme bildirimleri göndermek için kullanılan yöntem. **Gönder** yöntemi aşağıdaki parametreleri kabul eder:

* **Etiketleri** -etiket tanımlayıcısı. Hiçbir etiket sağlanırsa, tüm istemciler için bildirim gönderilir.
* **Yükü** -ileti JSON ya da dize yükü.
* **Geri çağırma** -geri çağırma işlevi.

Yük biçimi daha fazla bilgi için **bildirim yükü** bölümünü [yerel ve anında iletilen bildirim Programlama Kılavuzu](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) belge.

Aşağıdaki kod **ApnsService** örneği tarafından açığa çıkarılan **NotificationHubService** tüm istemcilere bir uyarı iletisi göndermek için:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
         // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Nasıl yapılır: Windows Phone uygulamalarına anında iletme bildirimleri gönderme
**MpnsService** nesnesi sağlayan bir **Gönder** Windows Phone uygulamalarına anında iletme bildirimleri göndermek için kullanılan yöntem. **Gönder** yöntemi aşağıdaki parametreleri kabul eder:

* **Etiketleri** -etiket tanımlayıcısı. Hiçbir etiket sağlanırsa, tüm istemciler için bildirim gönderilir.
* **Yükü** -ileti XML yükü.
* **TargetName**  -  `toast` kutlama bildirimleri için. `token` kutucuk bildirimleri için.
* **NotificationClass** -bildirim önceliğini. Bkz: **HTTP üstbilgi öğelerini** bölümünü [anında iletme bildirimleri bir sunucudan](https://msdn.microsoft.com/library/hh221551.aspx) belge için geçerli değerler.
* **Seçenekleri** : isteğe bağlı istek üst.
* **Geri çağırma** -geri çağırma işlevi.

Geçerli bir listesi için **TargetName**, **NotificationClass** ve üst bilgi seçeneklerini kullanıma [anında iletme bildirimleri bir sunucudan](https://msdn.microsoft.com/library/hh221551.aspx) sayfası.

Aşağıdaki örnek kod kullandığı **MpnsService** örneği tarafından açığa çıkarılan **NotificationHubService** bir anında iletme bildirimi göndermek için:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Nasıl yapılır: Evrensel Windows Platformu (UWP) uygulamaları için anında iletme bildirimleri gönderme
**WnsService** nesnesi sağlayan bir **Gönder** Evrensel Windows platformu uygulamaları için anında iletme bildirimleri göndermek için kullanılan yöntem.  **Gönder** yöntemi aşağıdaki parametreleri kabul eder:

* **Etiketleri** -etiket tanımlayıcısı. Hiçbir etiket sağlanırsa, kayıtlı tüm istemcilere bildirim gönderilir.
* **Yükü** -XML ileti yükü.
* **Tür** -bildirim türü.
* **Seçenekleri** : isteğe bağlı istek üst.
* **Geri çağırma** -geri çağırma işlevi.

Geçerli türler ve istek üstbilgileri listesi için bkz: [anında iletme bildirimi hizmeti istek ve yanıt üstbilgileri](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

Aşağıdaki kod **WnsService** örneği tarafından açığa çıkarılan **NotificationHubService** bir UWP uygulamasına bir anında iletme bildirimi göndermek için:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
         // notification sent
      }
    });

## <a name="next-steps"></a>Sonraki Adımlar
Yukarıdaki örnek kod, hizmet altyapısı, çok çeşitli cihazlara anında iletme bildirimleri göndermeyi kolayca oluşturmanızı sağlar. Node.js ile Notification hubs'ı kullanmanın temellerini öğrendiğinize göre bu özellikler daha fazla nasıl genişletebileceğiniz hakkında daha fazla bilgi için bu bağlantıları izleyin.

* MSDN işinize yarayacak [Azure Notification hubs'ı](https://msdn.microsoft.com/library/azure/jj927170.aspx).
* Ziyaret [Node için Azure SDK] GitHub deposunu daha fazla örnekleri ve uygulama ayrıntıları.

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
[SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
[SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
[Web Site with WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs
[webmatrix]: https://docs.microsoft.com/aspnet/web-pages/videos/introduction/create-a-website-using-webmatrix
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
[Azure Portal]: https://portal.azure.com
