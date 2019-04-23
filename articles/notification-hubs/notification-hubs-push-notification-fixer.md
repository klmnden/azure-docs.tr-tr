---
title: Azure Notification hubs'ı bırakılan bildirimler tanılayın
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
ms.date: 04/04/2019
ms.author: jowargo
ms.openlocfilehash: 4fc4175c03baa4ddb81507dd4001fcdbe7c7058b
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60149555"
---
# <a name="diagnose-dropped-notifications-in-azure-notification-hubs"></a>Azure Notification hubs'ı bırakılan bildirimler tanılayın

Azure Notification Hubs ile ilgili karşılaşılan bir soru, istemci cihazlarda uygulama bildirimleri görünmez sorunu gidermek şeklidir. Müşteriler, nerede ve neden bildirimleri bırakılan ve sorunun nasıl çözüleceğini öğrenmek ister. Bu makalede, bildirimleri bırakılan veya cihazlar tarafından alınmayan neden tanımlar. Ayrıca, kök nedenini belirlemek nasıl açıklar.

Bildirim hub'ları bildirimleri bir cihaza nasıl gönderim ilk anlamak için önemlidir.

![Notification Hubs mimarisi][0]

İletinin gönderildiği bir tipik send notification akışta *uygulama arka ucunun* bildirim hub'larına. Bildirim hub'ları tüm kayıtlar işler. Yapılandırılmış etiketleri ve hedefleri belirlemek için etiket ifadeleri dikkate alır. Anında iletme bildirimi alması gereken kayıtları hedeflerdir. Bu kayıtları herhangi biri, müşterilerimizin desteklenen platformlarda yayılabilir mi: Android, Baidu (Android cihazlar Çin'de), iOS, Windows ve Windows Phone yangın işletim sistemi (Amazon).

Oluşturulmuş hedefleri ile bildirimleri göndermek için Notification hubs'ı gönderim *anında iletilen bildirim servisi* cihaz platformu için. Apple anında iletilen bildirim servisi (APNs) Apple ve Firebase Cloud Messaging (FCM) için Google için verilebilir. Bildirim hub'ları bildirim bildirimleri kayıtları birden çok toplu iş arasında bölün. Altında Azure Portalı'nda ayarlamanız kimlik bilgilerine göre ilgili anında iletme bildirimi hizmeti ile kimlik doğrulaması **bildirim Hub'ı yapılandırma**. Anında iletme bildirimi hizmeti ardından ileten ilgili bildirimlere *istemci cihazları*.

Bildirim teslim son oluşturan platformun anında iletme bildirimi hizmeti ile cihaz arasındadır. (İstemci, uygulama arka ucu, bildirim hub'ları ve platformun anında iletilen bildirim servisi) anında iletme bildirimi işlem dört aşamada birini bildirim teslimi başarısız olabilir. Notification Hubs mimarisi hakkında daha fazla bilgi için bkz. [Notification Hubs'a genel bakış].

İlk kurulum sırasında bir hata bildirimleri göndermeyi oluşabilir test/hazırlama aşamasında. Bu aşamada bırakılan bildirim, bir yapılandırma sorunu gösterebilir. Üretimde bildirimi göndermek için bir hata oluşması durumunda bazı veya tüm bildirimleri bırakılabilir. Daha ayrıntılı bir uygulama veya Mesajlaşma deseni sorun bu durumda gösterilir.

Sonraki bölümde, bildirimler, ortak için seyrek değişen bırakılabilir senaryoları bakar.

## <a name="notification-hubs-misconfiguration"></a>Bildirim hub'ları hatalı yapılandırma ##

İlgili anında iletme bildirimi hizmeti için bildirimleri göndermek için Notification hubs'ı kendisi, uygulamanızın bağlamında doğrulaması gerekir. Hedef platformun bildirim hizmeti (Microsoft, Apple, Google, vb.) ile bir geliştirici hesabı oluşturmanız gerekir. Ardından, bir belirteç veya PNS hedef ile çalışmak için kullandığınız anahtarı nereden işletim sistemi ile uygulamanızı kaydetmeniz gerekir.

Azure portalında platformunuzun kimlik bilgileri eklemeniz gerekir. Hiç bildirim aygıtı ulaşıyor, ilk adım, doğru kimlik bilgileri, bildirim hub'ları yapılandırılır emin olmaktır. Kimlik bilgilerini bir platforma özgü Geliştirici hesabı altında oluşturduğunuz uygulamayı eşleşmesi gerekir.

Bu işlemi tamamlamak adım adım yönergeler için bkz: [Azure Notification Hubs ile çalışmaya başlama].

Denetlemek için bazı ortak yanlış yapılandırmalar şunlardır:

### <a name="notification-hub-name-location"></a>Bildirim hub'ı ad konumu

Bildirim hub'ı adınız (olmadan yazım yanlışları) bu konumların her biri aynı olduğundan emin olun:
   * İstemciden kayıt yeri
   * Burada bir arka uçtan bildirimler gönderin
   * Anında iletme bildirimi hizmet kimlik bilgilerini yapılandırdığınız yerde

İstemcide doğru paylaşılan erişim imzası yapılandırma dizeleri kullanın ve uygulamayı yeniden sona emin olun. Genellikle, kullanmalısınız **DefaultListenSharedAccessSignature** istemcide ve **DefaultFullSharedAccessSignature** uygulama arka ucu. Bu bildirimleri göndermek için Notification hubs'ı için izinler verir.

### <a name="apn-configuration"></a>APN yapılandırma ###

İki farklı hub'lar sürdürmeniz gerekir: üretim ve test için başka biri. Daha sonra üretim ortamında kullanacaksanız sertifika/hub ayrı hub'ına bir korumalı alan ortamında kullandığınız sertifika yüklemeniz gerekir. Farklı türde sertifikaları aynı hub'ı yüklemeye çalışmayın. Bu bildirim hatalarına neden.

Aynı hub'ına yanlışlıkla farklı türde sertifikaları yüklerseniz, hub'ını silmek ve yeni bir hub ile yeni başlangıç gerekir. Herhangi bir nedenden dolayı hub silemiyorsanız, hub'ın en az var olan tüm kayıtları silmeniz gerekir.

### <a name="fcm-configuration"></a>FCM yapılandırma ###

1. Emin *sunucu anahtarı* Firebase eşleşmeler arasından Azure Portalı'nda kayıtlı sunucu anahtarı aldığınız.

   ![Firebase sunucu anahtarı][3]

2. Yapılandırdığınızdan emin olun **proje kimliği** istemci üzerinde. Değeri elde edebilirsiniz **proje kimliği** Firebase panosundan.

   ![Firebase proje kimliği][1]

## <a name="application-issues"></a>Uygulama sorunları ##

### <a name="tags-and-tag-expressions"></a>Etiket ve etiket ifadeleri ###

Hedef kitlenizi segmentlere ayırmak için etiketler veya etiket ifadeleri kullanırsanız, bildirimi gönderdiğinizde, hedef bulunması mümkündür. Bu hata belirtilen etiketleri veya etiket ifadeleri, gönderme çağrısında temel alır.

Bir bildirim gönderdiğinizde etiketlerle eşleşecek emin olmak için kayıtlarınızı gözden geçirin. Daha sonra bu kayıtları olan istemcilerden gelen bildirim alındığını doğrulayın.

Örneğin, "Siyaset" etiketi tüm kayıtlar Notification Hubs ile kullandığınızı düşünün Ardından "Spor" etiketli bir bildirim gönderirseniz, herhangi bir cihaza bildirim gönderilmez. Etiket ifadeleri nereye kaydettiğiniz "Etiket A" kullanarak karmaşık bir servis talebi gerektirebilir *veya* "Etiket A & & Etiket b", "Etiketi B", ancak hedeflenen Makalenin devamındaki kendi kendine tanılama ipuçları bölümüne kayıtlarınızı ve bunların etiketleri gözden geçirmek nasıl gösterir.

### <a name="template-issues"></a>Şablon sorunlarını ###

Şablonları kullanıyorsanız, açıklanan yönergeleri izlediğinizden emin olun [Şablonlar].

### <a name="invalid-registrations"></a>Geçersiz kayıtlar ###

Bildirim hub'ı doğru yapılandırıldığından ve etiketleri veya etiket ifadeleri doğru şekilde kullanılan, geçerli hedefleri bulunamadı. Bildirimleri bu hedefe gönderilmelidir. Notification hubs'ı sonra ateşlenir paralel birkaç işleme toplu işlemi devre dışı. Her toplu işin bir kayıt kümesine iletileri gönderir.

> [!NOTE]
> Bildirim hub'ları toplu paralel olarak işlediğinden, bildirimleri teslim sırasını garanti edilmez.

Notification hubs'ı bir "en büyük bir kez" ileti teslim modeli için İyileştirildi. Bildirim yok bir cihaza birden çok kez teslim edilir, böylece yinelenen verileri kaldırma, denediğimiz. Kayıtları için anında iletme bildirimi hizmeti gönderilmeden önce cihaz tanımlayıcısı yalnızca bir ileti gönderilir emin olmak için denetlenir.

Her toplu işin hangi sırayla kabul eder ve kayıtları doğrular anında iletme bildirimi hizmetine gönderilir. Bu işlem sırasında anında iletme bildirimi hizmeti bir veya daha fazla kaydı bir hatayla batch'te algılar mümkündür. Anında iletme bildirimi hizmeti Notification hubs'ı daha sonra bir hata döndürür ve işlemi durdurur. Anında iletme bildirimi hizmeti toplu tamamen koparır. Bu bir TCP Akış Protokolü kullanan APNs ile birlikte, özellikle doğrudur.

Bu durumda, hatalı kayıt veritabanından kaldırıldı. Ardından, bildirim teslimi cihazları toplu geri kalanı için tekrar deneyeceğiz.

Bir kayıt karşı başarısız teslim denemesi hakkında daha fazla hata bilgisini almak için Notification Hubs REST API'lerini kullanabilirsiniz [ileti başına Telemetri: Bildirim iletisi telemetri alma](https://msdn.microsoft.com/library/azure/mt608135.aspx) ve [PNS geri bildirim](https://msdn.microsoft.com/library/azure/mt705560.aspx). Örnek kod için bkz: [Gönder REST örnek](https://github.com/Azure/azure-notificationhubs-dotnet/tree/master/Samples/SendRestExample/).

## <a name="push-notification-service-issues"></a>Anında iletme bildirimi hizmeti sorunları

Anında iletme bildirimi hizmeti bildirimi aldıktan sonra cihaza bildirimi teslim eder. Bu noktada, Notification Hubs, cihaz bildirim teslim üzerinde denetimi yoktur.

Platform bildirim hizmetlerine sağlam olduğundan, birkaç saniye içinde cihazlara ulaşmak için bildirimleri eğilimindedir. Anında iletme bildirimi Hizmeti azaltma, bildirim hub'ları bir üstel geri alma stratejisi uygular. Anında iletme bildirimi hizmeti 30 dakika boyunca ulaşılamaz kalırsa, süresinin dolmasını ve iletileri kalıcı olarak bırak yerinde bir ilke yoktur.

Bir bildirimi teslim etmek bir anında iletme bildirimi hizmeti çalışır, ancak cihaz çevrimdışı bildirimi anında iletme bildirim hizmeti tarafından depolanır. Bu süre yalnızca sınırlı bir süre için depolanır. Cihaz kullanıma sunulduğunda bildirim cihaza gönderilir.

Her uygulama, yalnızca bir son bildirim depolar. Bir cihaz çevrimdışı çalışırken birden çok bildirim gönderilen her yeni bir bildirim atılması sonuncu neden olur. Yalnızca en yeni bildirim tutma çağrılır *birleşim* , APNs ve *daraltma* FCM içinde. (FCM çöken bir anahtar kullanır.) Cihaz uzun bir süre için çevrimdışı kaldığında, cihaz için depolanan bildirimleri atılır. Daha fazla bilgi için [APNs genel bakış] ve [FCM iletileri hakkında].

Notification Hubs ile genel SendNotification API'sini kullanarak bir HTTP üst bir birleştirme anahtarı geçirebilirsiniz. Örneğin, .NET SDK için kullanacağınız `SendNotificationAsync`. SendNotification API, ayrıca ilgili anında iletme bildirimi hizmeti olarak geçirilen HTTP üstbilgilerini alır.

## <a name="self-diagnosis-tips"></a>Kendi kendine tanılama ipuçları

Bildirim hub'larındaki bırakılan bildirimler kök nedeni tanılamak için yolları aşağıda verilmiştir.

### <a name="verify-credentials"></a>Kimlik bilgilerini doğrulama ###

#### <a name="push-notification-service-developer-portal"></a>Anında iletme bildirimi hizmeti Geliştirici Portalı ####

(APNs, FCM, Windows bildirim hizmeti vb.) ilgili anında iletme bildirimi Hizmet Geliştirici Portalı'nda kimlik bilgilerini doğrulayın. Daha fazla bilgi için [Öğreticisi: Azure Notification Hubs'ı kullanarak, Evrensel Windows platformu uygulamaları için bildirimleri gönderecek](https://docs.microsoft.com/en-us/azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification).

#### <a name="azure-portal"></a>Azure portal ####

Gözden geçirin ve anında iletme bildirimi hizmeti Geliştirici Portalı'ndan elde kimlik bilgilerini eşleştirmek için Git **erişim ilkeleri** Azure portalında sekmesi.

![Azure portalında erişim ilkeleri][4]

### <a name="verify-registrations"></a>Kayıtlarını doğrulayın

#### <a name="visual-studio"></a>Visual Studio ####

Visual Studio'da Azure Notification Hubs dahil olmak üzere birden çok Azure hizmetleri görüntüle ve Yönet için Sunucu Gezgini aracılığıyla bağlanabilirsiniz. Bu kısayol, geliştirme/test ortamınız için özellikle yararlıdır.

![Visual Studio Sunucu Gezgini][9]

Görüntüleyebilir ve tüm kayıtlar hub'ında yönetin. Kayıtları platform, yerel veya şablon kayıt, etiket, anında iletme bildirimi hizmet tanımlayıcısı, kayıt kimliği ve sona erme tarihi tarafından kategorilere ayrılabilir. Bu sayfada bir kayıt de düzenleyebilirsiniz. Etiketleri düzenleme için özellikle yararlıdır.

Bildirim hub'ınıza sağ **Sunucu Gezgini**seçip **Tanıla**. 

![Visual Studio Sunucu Gezgini: Menü tanılayın](./media/notification-hubs-diagnosing/diagnose-menu.png)

Aşağıdaki sayfayı görürsünüz:

![Visual Studio: Tanılama sayfası](./media/notification-hubs-diagnosing/diagnose-page.png)

Geçiş **cihaz kayıtları** sayfası:

![Visual Studio: Cihaz kayıtları](./media/notification-hubs-diagnosing/VSRegistrations.png)

Kullanabileceğiniz **Test gönderimi** sayfasında test bildirim iletisi göndermek için:

![Visual Studio: Test Gönderimi](./media/notification-hubs-diagnosing/test-send-vs.png)

> [!NOTE]
> Yalnızca geliştirme/test sırasında ve kayıtları sınırlı sayıda kaydı düzenlemek için Visual Studio'yu kullanın. Toplu kayıtlarınızı düzenlemeniz gerekiyorsa, dışarı aktarma kullanmayı göz önünde bulundurun ve içe aktarma açıklanan kayıt işlevi [nasıl yapılır: Dışarı aktarmak ve kayıtları toplu değiştirmek](https://msdn.microsoft.com/library/dn790624.aspx).

#### <a name="service-bus-explorer"></a>Service Bus Explorer ####

Birçok müşteri kullanın [hizmet veri yolu Gezgini](https://github.com/paolosalvatori/ServiceBusExplorer) görüntüleme ve bunların notification hubs'ı yönetme. Hizmet veri yolu Gezgini açık kaynaklı bir projedir. 

### <a name="verify-message-notifications"></a>İleti bildirimlerini doğrulayın

#### <a name="azure-portal"></a>Azure portal ####

Bir hizmet arka ucu çalışır, altında zorunda kalmadan istemcilerinize bir test bildirimi göndermek için **destek + sorun giderme**seçin **Test gönderimi**.

![Azure'da gönderme işlevselliğini test etme][7]

#### <a name="visual-studio"></a>Visual Studio ####

Ayrıca, Visual Studio'dan test bildirimleri gönderebilirsiniz.

![Visual Studio'da gönderme işlevselliğini test etmek][10]

Visual Studio Sunucu Gezgini ile Notification hubs'ı kullanma hakkında daha fazla bilgi için şu makalelere bakın:

* [Cihaz kayıtları için bildirim hub'ları görüntüleme](https://docs.microsoft.com/en-us/previous-versions/windows/apps/dn792122(v=win.10))
* [Yakından bakış: Visual Studio 2013 güncelleştirme 2 RC ve Azure SDK 2.3]
* [Visual Studio 2013 güncelleştirme 3 ve Azure SDK 2.4 sürümünü Duyurusu]

### <a name="debug-failed-notifications-and-review-notification-outcome"></a>Hata ayıklama başarısız bildirimleri ve bildirim sonucunu gözden geçirin

#### <a name="enabletestsend-property"></a>EnableTestSend özelliği ####

Notification Hubs aracılığıyla bir bildirim gönderdiğinizde, bildirim başlangıçta kuyruğa alınır. Bildirim hub'ları doğru hedefleri belirler ve ardından anında iletme bildirimi hizmeti için bir bildirim gönderir. REST API veya istemci SDK'larından birini kullanıyorsanız, iade gönderme çağrınızın yalnızca ileti Notification Hubs ile kuyruğa alınan anlamına gelir. Bu bildirim hub'ları için anında iletme bildirimi hizmeti bildirim sonunda gönderildiğinde Öngörüler ne sağlamaz.

Bildiriminiz istemci cihazda gelmezse, bildirim hub'ları için anında iletme bildirimi hizmeti sunmak çalışırken bir hata oluşmuş olabilir. Örneğin, anında iletme bildirim hizmeti tarafından izin verilen maksimum yük boyutu aşabilir veya Notification hubs'ı yapılandırılmış kimlik bilgileri geçersiz olabilir.

Bildirim hizmeti hataları anında Öngörüler elde etmek için kullanabileceğiniz [EnableTestSend] özelliği. Bu özellik, portal ya da Visual Studio istemci sınama iletileri gönderirken otomatik olarak etkinleştirilir. Ayrıntılı hata ayıklama bilgileri görmek için bu özelliği kullanabilirsiniz ve ayrıca API'ler aracılığıyla. Şu anda, .NET SDK'yı kullanabilirsiniz. Bu sonunda tüm istemci SDK'ları için eklenecektir.

Kullanılacak `EnableTestSend` REST araması özelliğiyle adlı bir sorgu dizesi parametresi ekleme *test* gönderme aramanız sonuna. Örneğin:

```text
https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test
```

#### <a name="net-sdk-example"></a>.NET SDK'sı örneği ####

Yerel açılır (bildirim) bildirim göndermek için .NET SDK'sını kullanarak bir örnek aşağıda verilmiştir:

```csharp
NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
var result = await hub.SendWindowsNativeNotificationAsync(toast);
Console.WriteLine(result.State);
```

Yürütme sonunda `result.State` yalnızca durumları `Enqueued`. Sonuçları anında iletme bildiriminiz ne tüm bilgiler sağlamaz.

Ardından, kullanabileceğiniz `EnableTestSend` Boolean özelliği. Kullanım `EnableTestSend` özelliğini başlatır `NotificationHubClient` bildirim gönderildiğinde, bildirim hizmeti hataları bildirme hakkında ayrıntılı durumunu almak için. Gönderme çağrı önce bildirim hub'ları, anında iletme bildirimi hizmeti için bildirimi teslim etmek gerektiğinden döndürülecek ek zaman alır.

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

#### <a name="sample-output"></a>Örnek çıktı ####

```text
DetailedStateAvailable
windows
7619785862101227384-7840974832647865618-3
The Token obtained from the Token Provider is wrong
```

Bu ileti, Notification hubs'ı yapılandırılmış kimlik bilgilerinin geçersiz olduğunu ya da hub'ında kayıtlar ile ilgili bir sorun olduğunu gösterir. Bu kaydı silmek ve istemci kaydı iletiyi göndermeden önce yeniden oluşturma izin verin.

> [!NOTE]
> Kullanım `EnableTestSend` özelliği yoğun olarak kısıtlandı. Bu seçenek, yalnızca geliştirme/test ortamı ve kayıtları sınırlı sayıda kullanın. Hata ayıklama bildirimleri yalnızca 10 cihaza gönderilir. Ayrıca bir sınır yoktur işleme 10 dakikada bir hata ayıklama gönderir.

### <a name="review-telemetry"></a>Telemetri gözden geçirin ###

#### <a name="azure-portal"></a>Azure portal ####

Portalda, bildirim hub'ınıza tüm etkinlik hızlı bir genel bakış alabilirsiniz.

1. Üzerinde **genel bakış** sekme, platform tarafından kayıtlarını, bildirimleri ve hataları toplu bir görünümünü görebilirsiniz.

   ![Bildirim hub'ları Genel Bakış Panosu][5]

2. Üzerinde **İzleyici** sekmesinde, diğer birçok platforma özgü ölçümleri daha ayrıntılı bir bakış ekleyebilirsiniz. Anında iletme bildirimi hizmeti için bildirim göndermek Notification Hubs çalışır olduğunda, döndürülen hata özellikle göz atabilirsiniz.

   ![Azure portal etkinlik günlüğü][6]

3. Başlamak inceleyerek **gelen iletileri**, **kayıt işlemleri**, ve **başarılı bildirimler**. Ardından, anında iletme bildirimi hizmeti için özel hatalarını gözden geçirmek için platform başına sekmesine gidin.

4. Bildirim hub'ınız için kimlik doğrulama ayarlarını hatalıysa ileti **PNS kimlik doğrulama hatası** görünür. Anında iletme bildirimi hizmet kimlik bilgilerini kontrol etmek için iyi bir göstergesidir.

#### <a name="programmatic-access"></a>Programlı erişim ####

Programlı erişim hakkında daha fazla bilgi için bkz: [programlı erişim](https://docs.microsoft.com/en-us/previous-versions/azure/azure-services/dn458823(v=azure.100)).

> [!NOTE]
> Telemetri ile ilgili çeşitli özellikler verme ister ve kayıtları ve API'leri aracılığıyla erişim telemetri alma yalnızca standart hizmet katmanında kullanılabilir. Bu özellikleri ücretsiz veya temel hizmet katmanı kullanmayı denerseniz, SDK'sı kullanıyorsanız, bir özel durum iletisi alırsınız. REST API'lerini doğrudan özellikleri kullanırsanız, bir HTTP 403 (Yasak) hata iletisi alırsınız.
>
> Telemetri ile ilgili özellikleri kullanmak için ilk Azure portalında standart hizmet katmanı kullandığınızdan emin olun.  

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
[Şablonlar]: https://msdn.microsoft.com/library/dn530748.aspx
[APNs genel bakış]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html
[FCM iletileri hakkında]: https://firebase.google.com/docs/cloud-messaging/concept-options
[Export and modify registrations in bulk]: https://msdn.microsoft.com/library/dn790624.aspx
[Service Bus Explorer code]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[View device registrations for notification hubs]: https://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx
[Yakından bakış: Visual Studio 2013 güncelleştirme 2 RC ve Azure SDK 2.3]: https://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs
[Visual Studio 2013 güncelleştirme 3 ve Azure SDK 2.4 sürümünü Duyurusu]: https://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/
[EnableTestSend]: https://docs.microsoft.com/dotnet/api/microsoft.azure.notificationhubs.notificationhubclient.enabletestsend?view=azure-dotnet
[Programmatic telemetry access]: https://msdn.microsoft.com/library/azure/dn458823.aspx
