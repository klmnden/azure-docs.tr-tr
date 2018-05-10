---
title: Azure bildirim hub'ları ve Node.js ile anında iletme bildirimleri gönderme
description: Bir Node.js uygulamasından anında iletme bildirimleri göndermek için bildirim hub'ları kullanmayı öğrenin.
keywords: anında iletme bildirimi, anında iletme notifications,node.js itme ios anında iletme
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
ms.openlocfilehash: 7463d41382c59e4f7f03b58dbcbc3f5c45e9d15c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Azure bildirim hub'ları ve Node.js ile anında iletme bildirimleri gönderme
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a>Genel Bakış
> [!IMPORTANT]
> Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).
> 
> 

Bu kılavuz bir Node.js uygulamasından doğrudan Azure Notification Hubs Yardım ile anında iletme bildirimleri göndermeyi gösterir. 

Aşağıdaki platformlarda uygulamalarına anında iletme bildirimleri gönderme kapsamdaki senaryolar şunlardır:

* Android
* iOS
* Windows Phone
* Evrensel Windows platformu 

Bildirim hub'ları hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next) bölümü.

## <a name="what-are-notification-hubs"></a>Bildirim Hub'ları nedir?
Azure bildirim hub'ları, mobil cihazlara anında iletme bildirimleri göndermek için kullanımı kolay, çok platformlu, ölçeklenebilir bir altyapı sunar. Hizmet altyapısı hakkında daha fazla bilgi için bkz: [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) sayfası.

## <a name="create-a-nodejs-application"></a>Bir Node.js uygulaması oluşturma
Bu öğreticinin ilk adımı, yeni ve boş bir Node.js uygulaması oluşturmaktır. Bir Node.js uygulaması oluşturma ile ilgili yönergeler için bkz: [oluşturma ve bir Node.js uygulamasını Azure Web sitesini dağıtmak][nodejswebsite], [Node.js bulut hizmeti] [ Node.js Cloud Service] Windows PowerShell kullanarak veya [Web sitesini WebMatrix ile].

## <a name="configure-your-application-to-use-notification-hubs"></a>Uygulamanızı bildirim hub'ları kullanacak şekilde yapılandırma
Azure bildirim hub'ları kullanmak için karşıdan yükleme ve Node.js kullanma gerekir [azure paketi](https://www.npmjs.com/package/azure), anında iletme bildirimi REST Hizmetleri ile iletişim yardımcı kitaplıklar yerleşik bir kümesini içerir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Paket elde etmek için düğüm paketi Yöneticisi (NPM) kullanın
1. Bir komut satırı arabirimi gibi kullandığınız **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (Linux) ve boş uygulamanızı oluşturduğunuz klasöre gidin.
2. Tür **npm yükleme azure sb** komut penceresinde.
3. El ile çalıştırabilirsiniz **ls** veya **dir** doğrulamak için komutu bir **düğümü\_modülleri** klasörü oluşturuldu. Bu klasör içinde bulma **azure** bildirim hub'ı erişmek için gereken kitaplıklar içeren paket.

> [!NOTE]
> Üzerinde resmi NPM yükleme hakkında daha fazla bilgiyi [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 
> 
> 

### <a name="import-the-module"></a>Modülünü içeri aktarın
Bir metin düzenleyicisi kullanarak, en üstüne aşağıdakileri ekleyin **server.js** uygulamanın dosya:

    var azure = require('azure');

### <a name="set-up-an-azure-notification-hub-connection"></a>Azure bildirim hub'ı bağlantı kurma
**NotificationHubService** nesne notification hubs ile çalışmanıza olanak sağlar. Aşağıdaki kod oluşturur bir **NotificationHubService** nesne adlı bildirim hub'ı **hubname**. Üst kısmına ekleyin **server.js** azure modülü içeri aktarmak için deyimi sonra dosyayı:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

Bağlantı **connectionstring** değeri elde edilebilir gelen [Azure portal] aşağıdaki adımları gerçekleştirerek:

1. Sol gezinti bölmesinde **Gözat**.
2. Seçin **bildirim hub'ları**, örnek için kullanmak istediğiniz hub'ı bulun. Başvurabilirsiniz [Windows mağazası Başlarken Öğreticisi](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) yeni bir bildirim hub'ı oluşturma yardıma gereksinim duyarsanız.
3. Seçin **ayarları**.
4. Tıklayın **erişim ilkeleri**. Her iki paylaşılan ve tam erişim bağlantı dizeleri bakın.

![Azure portal - bildirim hub'ları](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> Kullanarak bağlantı dizenizi alabilirsiniz **Get-AzureSbNamespace** tarafından sağlanan cmdlet [Azure PowerShell](/powershell/azureps-cmdlets-docs) veya **azure sb ad alanı göster** komutunu [Azure komut satırı arabirimi (Azure CLI)](../cli-install-nodejs.md).
> 
> 

## <a name="general-architecture"></a>Genel mimari
**NotificationHubService** nesneyi kullanıma sunan belirli cihazlar ve uygulamalar için anında iletme bildirimleri göndermek için aşağıdaki nesne örnekleri:

* **Android** -kullanmak **GcmService** şu adresten edinilebilir nesne **notificationHubService.gcm**
* **iOS** -kullanmak **ApnsService** adresindeki erişilebilir olan nesne, **notificationHubService.apns**
* **Windows Phone** -kullanmak **MpnsService** şu adresten edinilebilir nesne **notificationHubService.mpns**
* **Evrensel Windows platformu** -kullanmak **WnsService** şu adresten edinilebilir nesne **notificationHubService.wns**

### <a name="how-to-send-push-notifications-to-android-applications"></a>Nasıl yapılır: Android uygulamalarına anında iletme bildirimleri gönderme
**GcmService** nesne sağlayan bir **Gönder** Android uygulamalarına anında iletme bildirimleri göndermek için kullanılan yöntem. **Gönder** yöntemi aşağıdaki parametreleri kabul eder:

* **Etiketler** -etiketi tanımlayıcısı. Hiçbir etiketi sağlanırsa, tüm istemcilere bildirim gönderilir.
* **Yükü** -ileti JSON veya ham dize yükü.
* **Geri çağırma** -geri çağırma işlevi.

Yük biçimi hakkında daha fazla bilgi için bkz: **yükü** bölümünü [uygulama GCM Server](http://developer.android.com/google/gcm/server.html#payload) belge.

Aşağıdaki kod **GcmService** örneği kullanıma sunulan **NotificationHubService** tüm kayıtlı istemcilere anında iletme bildirimi göndermek için.

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

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Nasıl yapılır: iOS uygulamalarına anında iletme bildirimleri gönderme
Yukarıda açıklanan Android uygulamaları ile aynı **ApnsService** nesnesi sağlar bir **Gönder** iOS uygulamalarına anında iletme bildirimleri göndermek için kullanılan yöntem. **Gönder** yöntemi aşağıdaki parametreleri kabul eder:

* **Etiketler** -etiketi tanımlayıcısı. Hiçbir etiketi sağlanırsa, tüm istemcilere bildirim gönderilir.
* **Yükü** -ileti JSON veya dize yükü.
* **Geri çağırma** -geri çağırma işlevi.

Yük biçimi daha fazla bilgi için bkz: **bildirim yükü** bölümünü [yerel ve anında iletilen bildirim Programlama Kılavuzu](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) belge.

Aşağıdaki kod **ApnsService** örneği kullanıma sunulan **NotificationHubService** tüm istemciler için bir uyarı iletisi göndermek için:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
         // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Nasıl yapılır: Windows Phone uygulamalarına anında iletme bildirimleri gönderme
**MpnsService** nesne sağlayan bir **Gönder** Windows Phone uygulamalarına anında iletme bildirimleri göndermek için kullanılan yöntem. **Gönder** yöntemi aşağıdaki parametreleri kabul eder:

* **Etiketler** -etiketi tanımlayıcısı. Hiçbir etiketi sağlanırsa, tüm istemcilere bildirim gönderilir.
* **Yükü** -ileti XML yükü.
* **TargetName**  -  `toast` bildirimleri için. `token` Döşeme bildirimleri için.
* **NotificationClass** -bildirim önceliğini. Bkz: **HTTP üstbilgi öğelerini** bölümünü [anında iletme bildirimleri bir sunucudan](http://msdn.microsoft.com/library/hh221551.aspx) belge için geçerli değerler.
* **Seçenekler** - isteğe bağlı istek üstbilgileri.
* **Geri çağırma** -geri çağırma işlevi.

Geçerli bir listesi için **TargetName**, **NotificationClass** ve üst bilgi seçeneklerini kullanıma [anında iletme bildirimleri bir sunucudan](http://msdn.microsoft.com/library/hh221551.aspx) sayfası.

Aşağıdaki örnek kod kullanır **MpnsService** örneği kullanıma sunulan **NotificationHubService** anında bildirim göndermek için:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Nasıl yapılır: Evrensel Windows Platformu (UWP) uygulamaları için anında iletme bildirimleri gönderme
**WnsService** nesne sağlayan bir **Gönder** Evrensel Windows platformu uygulamalarına anında iletme bildirimleri göndermek için kullanılan yöntem.  **Gönder** yöntemi aşağıdaki parametreleri kabul eder:

* **Etiketler** -etiketi tanımlayıcısı. Hiçbir etiketi sağlanırsa, tüm kayıtlı istemcilere bildirim gönderilir.
* **Yükü** -XML ileti yükü.
* **Tür** -bildirim türü.
* **Seçenekler** - isteğe bağlı istek üstbilgileri.
* **Geri çağırma** -geri çağırma işlevi.

Geçerli türler ve istek üstbilgileri listesi için bkz: [anında bildirim hizmeti istek ve yanıt üstbilgileri](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

Aşağıdaki kod **WnsService** örneği kullanıma sunulan **NotificationHubService** bir UWP uygulamasına anında iletme bildirim göndermek için:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
         // notification sent
      }
    });

## <a name="next-steps"></a>Sonraki Adımlar
Yukarıdaki örnek kod parçacıkları, çok çeşitli aygıtları için anında iletme bildirimleri göndermeyi hizmet altyapısı kolayca oluşturmanıza olanak tanır. Bildirim hub'ları node.js ile kullanma hakkında temel bilgileri öğrendiğinize göre bu özellikleri daha fazla nasıl genişletebileceğini hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

* MSDN başvuru için bkz: [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).
* Ziyaret [düğümü için Azure SDK] daha fazla örnekleri ve uygulama ayrıntıları için github'da depo.

[düğümü için Azure SDK]: https://github.com/WindowsAzure/azure-sdk-for-node
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
[Web sitesini WebMatrix ile]: /develop/nodejs/tutorials/web-site-with-webmatrix/
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
[Azure Portal]: https://portal.azure.com
