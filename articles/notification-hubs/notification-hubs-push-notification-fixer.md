---
title: Azure Notification Hubs - tanılama bırakılan bildirimler
description: Azure Notification hubs'ı bırakılan bildirimler ile ilgili yaygın sorunları tanılamayı öğrenin.
services: notification-hubs
documentationcenter: Mobile
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: c0fd7dec31a2c4054c59db3bae52cdb15ba01eed
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "57884430"
---
# <a name="azure-notification-hubs---diagnose-dropped-notifications"></a>Azure Notification Hubs - tanılama bırakılan bildirimler

Azure Notification hubs'ı müşterilerden en yaygın sorular istemci cihazlarında bir uygulamadan gönderilen bildirimleri görünmez sorunu gidermek nasıl biridir. Bildirimler, nerede ve neden bırakılan ve sorunun nasıl çözüleceğini öğrenmek isterler. Bu makalede, bildirimleri bırakılan veya cihazlar tarafından alınmayan neden tanımlar. Analiz edin ve kök nedenini öğrenin.

Notification Hubs hizmet bildirimleri bir cihaza nasıl gönderim ilk anlamak için önemlidir.

![Notification Hubs mimarisi][0]

İletinin gönderildiği bir tipik send notification akışta *uygulama arka ucunun* bildirim hub'larına. Bildirim hub'ları tüm kayıtlar üzerinde işlem yapar. İşleme yapılandırılmış etiketleri ve "hedef" belirlemek için etiket ifadeleri dikkate alır Anında iletme bildirimi alması gereken tüm kayıtlar hedeflerdir. Bu kayıtları herhangi yayılabilir veya bizim tüm desteklenen platformlarda: iOS, Google, Windows, Windows Phone, Kindle ve Baidu Çin Android için.

Oluşturulan hedefleriyle bildirimleri göndermek için Notification hubs'ı service gönderim *anında iletilen bildirim servisi* cihaz platformu için. Apple anında iletilen bildirim servisi (APNs) Apple ve Firebase Cloud Messaging (FCM) için Google için verilebilir. Bildirim hub'ları bildirim bildirimleri kayıtları birden çok toplu iş arasında bölün. Notification hubs'ın kimlik doğrulaması ile Azure portalında, altında ayarlanan kimlik bilgilerine göre ilgili anında iletme bildirimi hizmeti **bildirim Hub'ı yapılandırma**. Anında iletme bildirimi hizmeti ardından ileten ilgili bildirimlere *istemci cihazları*.

Bildirim teslim son oluşturan platform anında iletme bildirimi hizmeti ile cihaz arasındaki gerçekleşir. Dört ana bileşenleri (istemci, uygulama arka ucu, bildirim hub'ları ve platform anında iletilen bildirim servisi) anında iletme bildirimi işleminde bildirimleri bırakılmasına neden olabilir. Notification Hubs mimarisi hakkında daha fazla bilgi için bkz. [Notification Hubs'a genel bakış].

Bildirimleri teslim edememe, ilk kurulum sırasında oluşabilir test/hazırlama aşamasında. Bu aşamada bırakılan bildirim, bir yapılandırma sorunu gösterebilir. Üretimde bildirimleri göndermeyi hata oluşması durumunda bazı veya tüm bildirimleri bırakılabilir. Bu durumda, daha kapsamlı bir uygulama veya deseni sorunu ileti gösterilir.

Sonraki bölümde, bildirimler, ortak daha seyrek değişen bırakılabilir senaryoları bakar.

## <a name="notification-hubs-misconfiguration"></a>Bildirim hub'ları hatalı yapılandırma

Başarıyla ilgili anında iletme bildirimi hizmeti için bildirimleri göndermek için Notification Hubs hizmetinin geliştiricinin uygulama bağlamında kendi kimliğini doğrulamak gerekir. Bunun gerçekleşmesi için geliştirici ilgili platformun (Google, Apple, Windows vb.) bir geliştirici hesabı oluşturur. Ardından, geliştirici uygulamalarını kimlik bilgileri nereden platformuyla kaydeder.

Azure portalında platformunuzun kimlik bilgileri eklemeniz gerekir. Hiç bildirim aygıtı ulaşıyor, Notification hubs'ı doğru kimlik bilgilerini yapılandırıldığından emin olmak için ilk adım olmalıdır. Kimlik bilgilerini bir platforma özgü Geliştirici hesabı altında oluşturduğunuz uygulamayı eşleşmesi gerekir.

Bu işlemi tamamlamak adım adım yönergeler için bkz: [Azure Notification Hubs ile çalışmaya başlama].

Denetlemek için bazı ortak yanlış yapılandırmalar şunlardır:

**Genel:**

    * Bildirim hub'ı adınız (olmadan yazım yanlışları) bu konumların her biri aynı olduğundan emin olun:
        * Burada istemciden kaydedin.
        * Burada bir arka uçtan bildirimler gönderin.
        * Burada anında iletme bildirimi hizmeti kimlik bilgileri yapılandırılmamış.
    * İstemci ve uygulama arka ucunun doğru paylaşılan erişim imzası yapılandırma dizeleri kullandığınızdan emin olun. Genellikle, kullanmalısınız **DefaultListenSharedAccessSignature** istemcide ve **DefaultFullSharedAccessSignature** uygulamayı arka uç (bildirim göndermek için izinler verir Bildirim hub'ları).

**APNs yapılandırma:**

    You must maintain two different hubs: one hub for production, and another hub for testing. This means that you must upload the certificate that you use in a sandbox environment to a separate hub than the certificate and hub that you are going to use in production. Don't try to upload different types of certificates to the same hub. This might cause notification failures.

    If you inadvertently upload different types of certificates to the same hub, we recommend that you delete the hub and start fresh with a new hub. If for some reason you can't delete the hub, at a minimum, you must delete all the existing registrations from the hub.

**FCM yapılandırması:**

    1. Emin *sunucu anahtarı* Firebase aldığınız Azure Portalı'nda kayıtlı sunucu anahtarı eşleşir.

    ![Firebase sunucu anahtarı][3]

    2. Yapılandırdığınızdan emin olun **proje kimliği** istemci üzerinde. Değeri elde edebilirsiniz **proje kimliği** Firebase panosundan.

    ![Firebase proje kimliği][1]

## <a name="application-issues"></a>Uygulama sorunları

**Etiket ve etiket ifadeleri:**

    If you use tags or tag expressions to segment your audience, it's possible that when you send the notification, no target is found based on the tags or tag expressions that you specify in your send call.

    Review your registrations to ensure that there are matching tags when you send a notification. Then, verify the notification receipt only from the clients that have those registrations.

    As an example, if all your registrations with Notification Hubs were made by using the tag "Politics" and you send a notification with the tag "Sports," the notification isn't sent to any device. A complex case might involve tag expressions in which you registered by using "Tag A" OR "Tag B," but while sending notifications, you target "Tag A && Tag B." In the self-diagnosis tips section later in the article, we show you how to review your registrations and their tags.

** Şablon sorunlarını: **

    If you use templates, ensure that you follow the guidelines described in [Templates].

**Geçersiz kayıtlar:**

    If the notification hub was configured correctly, and if any tags or tag expressions were used correctly, valid targets are found. Notifications should be sent to these targets. The Notification Hubs service then fires off several processing batches in parallel. Each batch sends messages to a set of registrations.

    > [!NOTE]
    > Because processing is performed in parallel, the order in which the notifications are delivered is not guaranteed.

    Notification Hubs is optimized for an "at-most once" message delivery model. We attempt deduplication, so that no notifications are delivered more than once to a device. To ensure this, we check registrations and ensure that only one message is sent per device identifier before the message is sent to the push notification service.

    As each batch is sent to the push notification service, which in turn is accepting and validating the registrations, it's possible that the push notification service will detect an error with one or more of the registrations in a batch. In this case, the push notification service returns an error to Notification Hubs, and the process stops. The push notification service drops that batch completely. This is especially true with APNS, which uses a TCP stream protocol.

    We are optimized for at-most once delivery. But in this case, the faulting registration is removed from the database. Then, we retry notification delivery for the rest of the devices in that batch.

    To get more error information about the failed delivery attempt against a registration, you can use the Notification Hubs REST APIs [Per Message Telemetry: Get Notification Message Telemetry](https://msdn.microsoft.com/library/azure/mt608135.aspx) and [PNS feedback](https://msdn.microsoft.com/library/azure/mt705560.aspx). For sample code, see the [Send REST example](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample).

## <a name="push-notification-service-issues"></a>Anında iletme bildirimi hizmeti sorunları

Bu bildirim iletisi platform anında iletme bildirimi hizmeti tarafından alındıktan sonra cihaza bildirim teslim etmek için anında iletme bildirimi hizmeti sorumluluğundadır. Bu noktada, bildirim hub'ları hizmeti dışında bir resim ve sırada veya cihaza bildirim teslim üzerinde denetimi yoktur.

Platform bildirim hizmetlerine sağlam olduğundan, bildirimleri, anında iletme bildirimi hizmeti cihazlardan ulaşması birkaç saniye içinde eğilimindedir. Anında iletme bildirimi Hizmeti azaltma, bildirim hub'ları bir üstel geri alma stratejisi uygular. Anında iletme bildirimi hizmeti 30 dakika boyunca ulaşılamaz kalırsa, süresinin dolmasını ve bu iletileri kalıcı olarak bırak yerinde bir ilke sahibiz.

Bir bildirimi teslim etmek bir anında iletme bildirimi hizmeti çalışır, ancak cihaz çevrimdışı bildirim sınırlı bir süre için anında iletme bildirimi hizmeti tarafından depolanır. Cihaz kullanıma sunulduğunda bildirim cihaza gönderilir.

Her uygulama için yalnızca bir son bildirim depolanır. Bir cihaz çevrimdışı çalışırken birden çok bildirim gönderilen her yeni bir bildirim atılmak üzere önceki bildirim neden olur. Yalnızca en yeni bildirim tutma olarak adlandırılır *birleşim bildirimleri* APNs içinde ve *daraltma* FCM (hangi çöken bir anahtar kullanır) içinde. Cihaz uzun bir süredir çevrimdışı kalırsa, cihaz için depolanan tüm bildirimler atılır. Daha fazla bilgi için [APNs genel bakış] ve [FCM iletileri hakkında].

Azure Notification Hubs ile genel SendNotification API'sini kullanarak bir HTTP üst bir birleştirme anahtarı geçirebilirsiniz. Örneğin, .NET SDK için kullanacağınız `SendNotificationAsync`. SendNotification API olarak geçirilen bir HTTP üst bilgileri de alır-ilgili anında iletme bildirimi hizmeti.

## <a name="self-diagnosis-tips"></a>Kendi kendine tanılama ipuçları

Bildirim hub'larındaki bırakılan bildirim kök nedenini tanılamak için yollar şunlardır:

### <a name="verify-credentials"></a>Kimlik bilgilerini doğrulama

**Anında iletme bildirimi hizmeti Geliştirici Portalı:**

(APNs, FCM, Windows bildirim hizmeti vb.) ilgili anında iletme bildirimi Hizmet Geliştirici Portalı'nda kimlik bilgilerini doğrulayın. Daha fazla bilgi için [Azure Notification Hubs ile çalışmaya başlama].

**Azure portalı:**

Anında iletme bildirimi hizmeti Geliştirici Portalı, Azure portalında, aldığınız değerlerle kimlik bilgilerini gözden geçirin ve Git **erişim ilkeleri** sekmesi.

![Azure portalında erişim ilkeleri][4]

### <a name="verify-registrations"></a>Kayıtlarını doğrulayın

**Visual Studio:**

Geliştirme için Visual Studio kullanıyorsanız, Azure Notification Hubs dahil olmak üzere birden çok Azure hizmetleri görüntüle ve Yönet için Sunucu Gezgini aracılığıyla bağlanabilirsiniz. Bu, öncelikli olarak geliştirme/test ortamınız için kullanışlıdır.

![Visual Studio Sunucu Gezgini][9]

Görüntüleyebilir ve platform tarafından yerel kategorilere hub'ınızda, tüm kayıtları veya şablon kayıt, tüm etiketleri, anında iletme bildirimi hizmet tanımlayıcısı, kayıt kimliği ve sona erme tarihi yönetin. Bu sayfada bir kayıt de düzenleyebilirsiniz. Etiketleri düzenleme için özellikle yararlıdır.

![Visual Studio cihaz kayıtları][8]

> [!NOTE]
> Yalnızca geliştirme ve test sırasında ve kayıtları sınırlı sayıda kaydı düzenlemek için Visual Studio'yu kullanın. Toplu kayıtlarınızı düzenlemeniz gerekiyorsa, dışarı aktarma kullanmayı göz önünde bulundurun ve içe aktarma açıklanan kayıt işlevi [dışarı aktarma ve kayıtları toplu değiştirme](https://msdn.microsoft.com/library/dn790624.aspx).

**Hizmet veri yolu Gezgini:**

Birçok müşteri kullanın [Service Bus Explorer] görüntüleme ve kendi bildirim hub'ı yönetme. Hizmet veri yolu Gezgini açık kaynaklı bir projedir. Örnekler için bkz: [hizmet veri yolu Gezgini kod].

### <a name="verify-message-notifications"></a>İleti bildirimlerini doğrulayın

 **Azure portalı:**

Bir hizmet arka ucu çalışır, altında zorunda kalmadan istemcilerinize bir test bildirimi göndermek için **destek + sorun giderme**seçin **Test gönderimi**.

![Azure'da gönderme işlevselliğini test etme][7]

**Visual Studio:**

Ayrıca, Visual Studio'dan test bildirimleri gönderebilirsiniz.

![Visual Studio'da gönderme işlevselliğini test etmek][10]

Visual Studio Sunucu Gezgini ile Notification hubs'ı kullanma hakkında daha fazla bilgi için şu makalelere bakın:

* [Bildirim hub'ları için cihaz kayıtları görüntüle]
* [Yakından bakış: Visual Studio 2013 güncelleştirme 2 RC ve Azure SDK 2.3]
* [Visual Studio 2013 güncelleştirme 3 ve Azure SDK 2.4 sürümünü Duyurusu]

### <a name="debug-failed-notifications-and-review-notification-outcome"></a>Hata ayıklama başarısız bildirimleri ve bildirim sonucunu gözden geçirin

**`EnableTestSend` Özellik:**

Bildirim hub'ları aracılığıyla bir bildirim ilk kez gönderdiğinizde, bildirim Notification Hubs ile işleme için kuyruğa alınır. Bildirim hub'ları doğru hedefleri belirler ve ardından anında iletme bildirimi hizmeti için bir bildirim gönderir. REST API veya istemci SDK'larından birini kullanıyorsanız, gönderme çağrınızın başarılı dönüş yalnızca ileti Notification Hubs ile başarıyla kuyruğa olduğunu gösterir. Bildirim hub'ları için anında iletme bildirimi hizmeti iletinin sonunda gönderildiğinde ne tüm bilgiler yok.

Bildiriminiz istemci cihazda gelmezse, Notification hubs'ın anında iletilen bildirim servisi için ileti teslim çalışırken bir hata oluştu mümkündür. Örneğin, anında iletme bildirim hizmeti tarafından izin verilen maksimum yük boyutu aşabilir veya Notification hubs'ı yapılandırılmış kimlik bilgileri geçersiz olabilir.

Bildirim hizmeti hataları anında Öngörüler elde etmek için kullanabileceğiniz [EnableTestSend] özelliği. Bu özellik, portal ya da Visual Studio istemci sınama iletileri gönderirken otomatik olarak etkinleştirilir. Ayrıntılı hata ayıklama bilgileri görmek için bu özelliği kullanabilirsiniz. API'ler aracılığıyla özelliğini de kullanabilirsiniz. Şu anda, .NET SDK'yı kullanabilirsiniz. Sonuç olarak, tüm istemci SDK'ları için eklenir.

Kullanılacak `EnableTestSend` REST araması özelliğiyle adlı bir sorgu dizesi parametresi ekleme *test* gönderme aramanız sonuna. Örneğin:

```text
https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test
```

**Örnek (.NET SDK):**

Yerel açılır (bildirim) bildirim göndermek için .NET SDK'sını kullanarak bir örnek aşağıda verilmiştir:

```csharp
NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
var result = await hub.SendWindowsNativeNotificationAsync(toast);
Console.WriteLine(result.State);
```

Yürütme sonunda `result.State` yalnızca durumları `Enqueued`. Sonuçları anında iletme bildiriminiz ne tüm bilgiler sağlamaz.

Ardından, kullanabileceğiniz `EnableTestSend` Boolean özelliği. Kullanım `EnableTestSend` özelliğini başlatır `NotificationHubClient` bildirim gönderildiğinde, bildirim hizmeti hataları bildirme hakkında ayrıntılı durumunu almak için. Gönderme çağrı yalnızca Notification hubs'ı bildirim sonucunu belirlemek için anında iletme bildirimi hizmeti için teslim ettiğini sonra döndürdüğü için döndürülecek ek zaman alır.

```csharp
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);

    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);

    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }
```

**Örnek çıktı:**

```text
DetailedStateAvailable
windows
7619785862101227384-7840974832647865618-3
The Token obtained from the Token Provider is wrong
```

Bu ileti, ya da geçersiz kimlik bilgileri, bildirim hub'ları yapılandırılmış veya hub'ında kayıtları bir sorun gösterir. Bu kaydı silmek ve istemci kaydı iletiyi göndermeden önce yeniden oluşturma izin öneririz.

> [!NOTE]
> Kullanım `EnableTestSend` özelliği yoğun olarak kısıtlandı. Bu seçenek, yalnızca geliştirme/test ortamı ve sınırlı sayıda kayıtlar ile kullanın. Yalnızca 10 cihazları için hata ayıklama bildirimleri göndereceğiz. Hata ayıklama gönderen 10 dakika başına işlemenin bir sınır de sahibiz.

### <a name="review-telemetry"></a>Telemetri gözden geçirin

**Azure portalını kullanın:**

Portalda, bildirim hub'ınıza tüm etkinlik hızlı bir genel bakış alabilirsiniz.

1. Üzerinde **genel bakış** sekme, platform tarafından kayıtlarını, bildirimleri ve hataları toplu bir görünümünü görebilirsiniz.

    ![Bildirim hub'ları Genel Bakış Panosu][5]

2. Üzerinde **İzleyici** sekmesinde, diğer birçok platforma özgü ölçümleri daha ayrıntılı bir bakış ekleyebilirsiniz. Bildirim hub'ları hizmeti için anında iletme bildirimi hizmeti Bildirim göndermeye çalıştığında döndürülen anında iletme bildirimi hizmeti ile ilgili özel hataları göz atabilirsiniz.

    ![Azure portal etkinlik günlüğü][6]

3. Başlamak inceleyerek **gelen iletileri**, **kayıt işlemleri**, ve **başarılı bildirimler**. Ardından, anında iletme bildirimi hizmeti için özel hatalarını gözden geçirmek için platform başına sekmesine gidin.

4. Bildirim hub'ınız için kimlik doğrulama ayarlarını hatalıysa ileti **PNS kimlik doğrulama hatası** görünür. Anında iletme bildirimi hizmet kimlik bilgilerini kontrol etmek için iyi bir göstergesidir.

* **Programlı erişim**

Programlı erişim hakkında daha fazla bilgi için bkz: [telemetri programlı erişim]

> [!NOTE]
> Telemetri ile ilgili çeşitli özellikler verme ister ve kayıtları ve API'leri aracılığıyla erişim telemetri alma yalnızca standart hizmet katmanında kullanılabilir. Kullanmayı denerseniz, bu özellikleri ücretsiz veya temel hizmet katmanını, REST API'lerini doğrudan özellikleri kullanırsanız SDK'sı ve HTTP 403 (Yasak) hata kullanırsanız, bir özel durum iletisi alırsınız.
>
> Telemetri ile ilgili özellikleri kullanmak için öncelikle standart hizmet katmanı kullanarak Azure portalında olun.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/FCMConfigure.png
[3]: ./media/notification-hubs-diagnosing/FCMServerKey.png
[4]: ../../includes/media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalAnalytics.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png

<!-- LINKS -->
[Notification Hubs'a genel bakış]: notification-hubs-push-notification-overview.md
[Azure Notification Hubs ile çalışmaya başlama]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Templates]: https://msdn.microsoft.com/library/dn530748.aspx
[APNs genel bakış]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html
[FCM iletileri hakkında]: https://firebase.google.com/docs/cloud-messaging/concept-options
[Export and modify registrations in bulk]: https://msdn.microsoft.com/library/dn790624.aspx
[Service Bus Explorer]: https://msdn.microsoft.com/library/dn530751.aspx#sb_explorer
[Hizmet veri yolu Gezgini kod]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Bildirim hub'ları için cihaz kayıtları görüntüle]: https://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx
[Yakından bakış: Visual Studio 2013 güncelleştirme 2 RC ve Azure SDK 2.3]: https://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs
[Visual Studio 2013 güncelleştirme 3 ve Azure SDK 2.4 sürümünü Duyurusu]: https://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/
[EnableTestSend]: https://docs.microsoft.com/dotnet/api/microsoft.azure.notificationhubs.notificationhubclient.enabletestsend?view=azure-dotnet
[Telemetri programlı erişim]: https://msdn.microsoft.com/library/azure/dn458823.aspx
