---
title: Azure bildirim hub'ları bildirim tanılama bırakıldı
description: Azure Notification Hubs bırakılan bildirimler ile ilgili genel sorunları tanılamak öğrenin.
services: notification-hubs
documentationcenter: Mobile
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: bc9ef70560f0485da81c1f54aa955cee76d280ab
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="diagnose-dropped-notifications-in-notification-hubs"></a>Bildirim hub'ları bırakılan bildirimler tanılama

Azure Notification Hubs müşterilerden en yaygın sorular bir uygulamadan gönderilen bildirimleri istemci aygıtlarda görünmüyor sorunu gidermek nasıl biridir. Bunlar nerede ve neden bildirimleri bırakıldı ve sorun gidermeye yönelik bilmek ister. Bu makalede, bildirimleri bırakılan veya cihazlar tarafından alınmayan neden tanımlar. Analiz etmek ve kök nedenini belirlemek öğrenin. 

İlk bildirim hub'ları service bildirimleri bir aygıta nasıl iter anlaşılması önemlidir.

![Bildirim hub'ları mimarisi][0]

Tipik gönderme bildirim akışı ileti gönderilen *uygulama arka ucu* bildirim hub'ları için. Bildirim hub'ları tüm kayıtlar üzerinde bir işlem yapar. Yapılandırılmış etiketleri ve "hedefleri." belirlemek için etiket ifadeleri işleme dikkate alır Hedefleri anında iletme bildirimi alması gereken tüm kayıtlar ' dir. Bu kayıtlar herhangi yayılabilir veya bizim desteklenen tüm platformlar: iOS, Google, Windows, Windows Phone, Kindle ve Baidu Çin Android için.

Bildirimleri göndermek için Notification Hubs hizmeti kurulan hedefleri ile iter *anında bildirim hizmeti* cihaz platformu için. Apple anında iletilen bildirim servisi (APNs) Apple ve Firebase bulut Mesajlaşma (FCM) için Google için örnekler. Bildirim hub'ları gönderim bildirimlerini kayıtlar birden çok toplu işlemi bölün. Bildirim hub'ları kimliğini doğrular Azure portalında altında ayarlanan kimlik bilgilerine göre ilgili anında bildirim hizmeti ile **bildirim Hub'ı yapılandırma**. Anında iletme bildirimi hizmet ardından ileten ilgili bildirimlere *istemci cihazları*. 

Son Bacak bildirim teslimi için platform anında bildirim hizmeti ve aygıt arasında yer alır. Anında iletme bildirimi işleminde (istemci, uygulama arka ucu, bildirim hub'ları ve platform anında bildirim hizmeti) dört ana bileşen hiçbirini bildirimleri bırakılmasına neden olabilir. Bildirim hub'ları mimarisi hakkında daha fazla bilgi için bkz: [Notification Hubs'a genel bakış].

Bildirimleri göndermeyi hata sırasında ilk oluşabilir test/hazırlama aşamasında. Bu aşamada bırakılan bildirimler bir yapılandırma sorunu gösteriyor olabilir. Üretimde bildirimleri göndermeyi hatası oluşursa, bazı veya tüm bildirimler bırakılabilir. Bu durumda, daha derin bir uygulama veya desen sorunu ileti gösterilir. 

Sonraki bölümde, bildirimler, ortak daha seyrek değişen bırakılabilir senaryoları bakar.

## <a name="notification-hubs-misconfiguration"></a>Bildirim hub'ları yanlış yapılandırma
Başarıyla ilgili anında bildirim hizmeti bildirimleri göndermek için geliştiricinin uygulama bağlamında kendi kimliğini doğrulamak Notification Hubs hizmeti gerekir. Bunun gerçekleşmesi için geliştirici ilgili platformun (Google, Apple, Windows ve benzeri) bir geliştirici hesabı oluşturur. Ardından, geliştirici kendi uygulama kimlik bilgileri nereden platformuyla kaydeder. 

Azure portalına platform kimlik bilgilerini eklemeniz gerekir. Hiçbir bildirim aygıtı ulaşıyor, doğru kimlik bilgilerini Notification Hubs ' yapılandırıldığından emin olmak için ilk adım olmalıdır. Kimlik bilgileri bir platforma özgü Geliştirici hesabı altında oluşturulan uygulama eşleşmesi gerekir. 

Bu işlemi tamamlamak adım adım yönergeler için bkz: [Azure Notification Hubs ile çalışmaya başlama].

Bazı ortak yapılandırma hataları denetlemek için şunlardır:

* **Genel**
   
    * Bildirim hub adınızı (olmadan, yazım hatalarını) bu konumlardan her birindeki aynı olduğundan emin olun:

        * Burada istemciden kaydedin.
        * Burada arka uçtan bildirimleri gönderin.
        * Burada anında bildirim hizmeti kimlik bilgileri yapılandırılmamış.
    
    * İstemci ve uygulama arka ucunun doğru paylaşılan erişim imzası yapılandırma dizeleri kullandığınızdan emin olun. Genellikle, kullanmalısınız **DefaultListenSharedAccessSignature** istemcide ve **DefaultFullSharedAccessSignature** uygulamanın arka uç (bildirimleri gönderme izinleri verir Bildirim hub'ları).

* **APNs yapılandırma**
   
    İki farklı hub bakımını yapmanız gerekir: üretim için bir hub ve test etmek için başka bir hub. Başka bir deyişle, bir korumalı alan ortamıdır sertifika ve üretimde kullanacaksanız hub'ı daha ayrı hub'ına kullandığınız sertifika yüklemeniz gerekir. Farklı türde sertifikaları aynı hub'ı yüklemeye çalışmayın. Bu bildirim hatasına neden olabilir. 
    
    Farklı türde sertifikaları yanlışlıkla aynı hub'ına yüklerseniz, hub'ı silin ve yeni bir hub ile yeni başlangıç öneririz. Bazı nedenlerle en azından, hub silemiyorsanız hub'dan varolan tüm kayıtlarını silmeniz gerekir. 

* **FCM yapılandırma** 
   
    1. Emin *sunucu anahtarı* Firebase alınan Azure Portalı'nda kayıtlı sunucu anahtarı eşleşir.
   
    ![Firebase sunucu anahtarı][3]
   
    2. Yapılandırdığınızdan emin olun **proje kimliği** istemci üzerinde. Değeri elde edebilirsiniz **proje kimliği** Firebase panosundan.
   
    ![Firebase proje kimliği][1]

## <a name="application-issues"></a>Uygulama sorunları
* **Etiketleri ve etiket ifadeleri**

    Kullanarak hedef kitlenizi segmentlere etiketleri veya etiket ifadeleri kullanıyorsanız bildirimi gönderdiğinizde, hiçbir hedef etiketler veya gönderme aramanız belirttiğiniz etiket ifadeleri göre bulunması mümkündür. 
    
    Bir bildirim gönderdiğinizde eşleşen etiketleri olduğundan emin olmak için kayıtları gözden geçirin. Ardından, bu kayıtları sahip istemcilerden gelen bildirim giriş doğrulayın. 
    
    Örnek olarak, tüm kayıtlar Notification Hubs ile "Siyaset" etiketini kullanarak yapılan ve "Spor" etiketi içeren bir bildirim gönderirseniz bildirim için herhangi bir CİHAZDAN gönderilen değil. Karmaşık bir servis talebi "Etiketi A" veya "Etiketi B" kullanarak kayıtlı etiket ifadeleri gerektirebilir, ancak bildirimleri gönderirken "Etiketi A & & etiketi b" hedef Self-diagnosis ipuçları bölümünde makalenin sonraki bölümlerinde, kayıtlar ve etiketlerini gözden geçirmek nasıl gösteriyoruz. 

* **Şablon sorunları**

    Bölümünde açıklanan yönergeleri izleyin şablonları kullanıyorsanız olun [şablonları]. 

* **Geçersiz kayıtlar**

    Bildirim hub'ı doğru bir şekilde yapılandırdıysanız ve herhangi bir etiket veya etiket ifadeleri doğru kullanıldıysa, geçerli hedef bulunamadı. Bildirimleri bu hedefe gönderilmelidir. Bildirim hub'ları service sonra birkaç işlem toplu paralel olarak devre dışı etkinleşir. Her toplu kayıt kümesine iletileri gönderir. 

    > [!NOTE]
    > Paralel işleme gerçekleştirildiğinden, bildirimleri teslim edilir sırayı garanti edilmez. 

    Bildirim hub'ları "konumundaki çoğu kez" ileti teslim modeli için iyileştirilmiştir. Hiçbir bildirim bir cihaza birden çok kez teslim edilir böylece biz yinelenenleri kaldırma, çalışır. Bu emin olmak için kayıtlar denetleyin ve iletisi için anında iletilen bildirim servisi gönderilmeden önce cihaz tanımlayıcısı yalnızca bir ileti gönderilir emin olun. 

    Her toplu sırayla kabul etmek ve kayıtları doğrulanıyor, anında iletme bildirimi hizmete gönderilen anında bildirim hizmeti bir veya daha fazla kayıtlar hatayla bir toplu işlemde algılar mümkündür. Bu durumda, anında bildirim hizmeti bildirim hub'ları için bir hata döndürür ve işlemi durdurur. Anında bildirim hizmeti toplu tamamen koparır. Bu, özellikle bir TCP akışı protokolünü kullanan APNS ile geçerlidir. 

    Biz adresindeki-en iyi duruma getirilmiş kez teslim. Ancak bu durumda, hataya neden olan kayıt veritabanından kaldırılır. Ardından, biz toplu aygıtları geri kalanı için bildirim teslimi yeniden deneyin.

    Bir kayıt karşı başarısız teslim girişimi hakkında daha fazla hata bilgi almak için bildirim hub'ları REST API'ler kullanabilirsiniz [başına ileti Telemetri: bildirim iletisi Telemetri almak](https://msdn.microsoft.com/library/azure/mt608135.aspx) ve [PNS geri bildirim](https://msdn.microsoft.com/library/azure/mt705560.aspx). Örnek kod için bkz: [Gönder REST örnek](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample).

## <a name="push-notification-service-issues"></a>Anında bildirim hizmeti sorunları
Bu bildirim iletisi platform anında bildirim hizmeti tarafından alınan sonra cihaza bildirimi teslim etmek anında bildirim hizmeti sorumluluğundadır. Bu noktada, bildirim hub'ları hizmeti dışında resim ve veya bildirim cihaza teslim olsa üzerinde denetimi yoktur. 

Platform Bildirim Hizmetleri sağlam olduğundan, birkaç saniye içinde anında iletme bildirimi hizmetinden cihazlara ulaşmak için bildirimler eğilimlidir. Anında bildirim hizmeti azaltma, bildirim hub'ları bir üstel geri alma stratejisi geçerlidir. Anında bildirim hizmeti 30 dakika boyunca ulaşılamaz kalırsa, biz süresinin dolmasını ve bu iletileri kalıcı olarak bırakma yerinde bir ilkeye sahip. 

Bir bildirim teslim etmek bir anında bildirim hizmeti çalışır, ancak cihaz çevrimdışıysa, bildirim sınırlı bir süre için anında bildirim hizmeti tarafından depolanır. Aygıt kullanılabilir hale geldiğinde bildirim cihaza teslim edilir. 

Her uygulama için yalnızca bir son bildirim depolanır. Bir aygıt çevrimdışı durumdayken birden fazla bildirim gönderiyorsanız, her bir yeni bildirim atılmak üzere önceki bildirim neden olur. Yalnızca en yeni bildirim tutma olarak adlandırılır *birleştirmesi bildirimleri* APNs içinde ve *daraltma* (hangi çöken bir anahtar kullanan) FCM içinde. Cihaz uzun bir süredir çevrimdışı kalırsa, cihaz için depolanan tüm bildirimler atılır. Daha fazla bilgi için bkz: [APNs genel bakış] ve [hakkında FCM iletileri].

Azure Notification Hubs ile genel SendNotification API'sini kullanarak HTTP üstbilgisinin birleştirmesi anahtar geçirebilirsiniz. Örneğin, .NET SDK için kullanacağınız **SendNotificationAsync**. SendNotification API ayrıca olarak geçirilen HTTP üstbilgileri alır-için ilgili anında iletme bildirimi hizmetidir. 

## <a name="self-diagnosis-tips"></a>Self-Diagnosis ipuçları
Bildirim hub'ları bırakılan bildirimler kök nedenini tanılamak için yollar şunlardır:

### <a name="verify-credentials"></a>Kimlik bilgilerini doğrulama
* **Anında iletme bildirimi hizmeti Geliştirici Portalı**
   
    (APNs, FCM, Windows bildirim hizmeti vb.) ilgili anında iletme bildirimi Hizmet Geliştirici Portalı'nda kimlik bilgilerini doğrulayın. Daha fazla bilgi için bkz: [Azure Notification Hubs ile çalışmaya başlama].

* **Azure Portal**
   
    Gözden geçirmek ve kimlik bilgilerinin anında bildirim hizmeti Geliştirici Portalı, Azure portalında edindiğiniz olanla eşleşmesi için Git **erişim ilkeleri** sekmesi. 
   
    ![Azure portalına erişim ilkeleri][4]

### <a name="verify-registrations"></a>Kayıtlar doğrulayın
* **Visual Studio**
   
    Geliştirme için Visual Studio kullanırsanız, görüntülemek ve bildirim hub'ları da dahil olmak üzere birden çok Azure hizmetleri yönetmek için Sunucu Gezgini üzerinden Azure bağlanabilirsiniz. Bu, geliştirme ve test ortamınız için özellikle yararlıdır. 
   
    ![Visual Studio Sunucu Gezgini][9]
   
    Görüntüleyin ve platformu tarafından yerel kategorilere hub'ınızdaki, tüm kayıtlar veya şablon kayıt, tüm etiketleri, anında bildirim hizmeti tanımlayıcısı, kayıt kimliği ve sona erme tarihi yönetin. Bu sayfada bir kaydı da düzenleyebilirsiniz. Etiketleri düzenleme için özellikle yararlıdır. 
   
    ![Visual Studio cihaz kayıtları][8]
   
   > [!NOTE]
   > Kayıtlar yalnızca geliştirme ve test sırasında ve kayıtlar sınırlı sayıda ile düzenlemek için Visual Studio kullanın. Toplu, kayıtları düzenlemeniz gerekiyorsa, dışa aktarma kullanmayı düşünün ve içe aktarma bölümünde açıklanan kayıt işlevi [dışarı aktarma ve toplu kayıtları değiştirme](https://msdn.microsoft.com/library/dn790624.aspx).
   > 
   > 
* **Service Bus Explorer**
   
    Birçok müşteri kullanmak [hizmet veri yolu Gezgini] görüntüleme ve kendi bildirim hub'ı yönetme. Hizmet veri yolu Gezgini açık kaynaklı bir projedir. Örnekler için bkz: [hizmet veri yolu Gezgini kod].

### <a name="verify-message-notifications"></a>İleti bildirimlerini doğrulayın
* **Azure Portal**
   
    Bir hizmet arka uç çalışır, altında gerek kalmadan istemcilerinize bir test bildirimi göndermek için **destek + sorun giderme**seçin **Test gönderimi**. 
   
    ![Azure'da test gönderme işlevi][7]
* **Visual Studio**
   
    Ayrıca Visual Studio'dan test bildirimleri gönderebilirsiniz.
   
    ![Visual Studio'da gönderme işlevselliğini sınamak][10]
   
    Bildirim hub'ları ile Visual Studio Sunucu Gezgini kullanma hakkında daha fazla bilgi için bu makalelere bakın: 
   
   * [Bildirim hub'ları için cihaz kayıtları görüntüle]
   * [Derin Dalış: Visual Studio 2013 güncelleştirme 2 RC ve Azure SDK 2.3]
   * [Visual Studio 2013 güncelleştirme 3'ü ve Azure SDK 2.4 sürümü Duyurusu]

### <a name="debug-failed-notifications-and-review-notification-outcome"></a>Başarısız bildirimleri hata ayıklama ve bildirim sonucunu gözden geçirin
**EnableTestSend özelliği**

Bildirim hub'ları aracılığıyla bir bildirim başlangıçta gönderdiğinizde, bildirim bildirim hub'ları işlemek için sıraya alındı. Bildirim hub'ları doğru hedefler belirler ve anında iletilen bildirim servisi için bildirim gönderir. REST API veya istemci SDK birini kullanıyorsanız, gönderme aramanız başarılı dönüş yalnızca ileti Notification Hubs ile başarıyla sıraya olduğunu anlamına gelir. Bildirim hub'ları için anında iletilen bildirim servisi ileti sonunda gönderildiğinde ne içine herhangi Insight yok. 

Bildiriminizin istemci aygıtta ulaşırsa değil, bildirim hub'ları için anında iletilen bildirim servisi ileti teslim çalışırken bir hata oluştu mümkündür. Örneğin, yükü boyutu anında bildirim hizmeti tarafından izin verilen en büyük değeri aşıyor ya da Notification Hubs ' yapılandırılan kimlik bilgileri geçersiz olabilir. 

Bildirim hizmeti hataları gönderme hakkında bilgi almak için kullanabileceğiniz [EnableTestSend] özelliği. Bu özellik, portal ya da Visual Studio istemcisi sınama iletileri gönderirken otomatik olarak etkinleştirilir. Ayrıntılı hata ayıklama bilgilerini görmek için bu özelliği kullanın. API'leri aracılığıyla özelliğini de kullanabilirsiniz. Şu anda, .NET SDK'ın olarak kullanabilirsiniz. Sonuç olarak, tüm istemci SDK'ları için eklenir. 

Kullanılacak **EnableTestSend** REST çağrısı özelliğiyle append adlı bir sorgu dizesi parametresi *test* gönderme aramanız sonuna. Örneğin: 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

**Örnek (.NET SDK)**

Yerel açılır (bildirim) bildirim göndermek için .NET SDK kullanarak bir örnek aşağıda verilmiştir:

```csharp
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);
```

Yürütme işleminin sonunda **sonucu. Durumu** yalnızca durumları **sıraya alınan**. Sonuçları, anında iletme bildirimi ne içine herhangi Insight sağlamaz. 

Ardından, kullanabileceğiniz **EnableTestSend** Boolean özelliği. Kullanım **EnableTestSend** , başlatma sırasında özelliği **NotificationHubClient** bildirim gönderildiğinde oluşan bildirim hizmeti hataları gönderme hakkında ayrıntılı bir durum bilgisi almak için. Gönderme çağrı yalnızca bildirim hub'ları bildirim sonucunu belirlemek için anında iletilen bildirim servisi teslim sonra döndürdüğü döndürmek için ek bir zaman alır. 

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

**Örnek çıktı**

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong

Bu ileti ya da geçersiz kimlik bilgileri bildirim hub'ları içinde yapılandırılmış olan veya kayıtlar hub ile ilgili bir sorun var. gösterir. Bu kayıt silme ve iletiyi göndermeden önce kayıt yeniden oluşturma istemci izin öneririz. 

> [!NOTE]
> Kullanımı **EnableTestSend** özelliği yoğun olarak azaltıldı. Bu seçenek, yalnızca geliştirme ve test ortamında ve kayıtlar sınırlı sayıda ile kullanın. Biz hata ayıklama bildirimlerini yalnızca 10 cihazlara gönderme. Biz de hata ayıklama gönderir 10 dakika başına işlemenin sınırlaması vardır. 
> 
> 

### <a name="review-telemetry"></a>Telemetri gözden geçirin
* **Azure portal’ı kullanma**
   
    Portalda, bildirim hub'ınıza tüm etkinliklerin hızlı bir genel bakış elde edebilirsiniz. 
   
    1. Üzerinde **genel bakış** sekme, platform tarafından kayıtlar, bildirimleri ve hataları birleşik bir görünümünü görebilirsiniz. 
   
        ![Bildirim hub'ları Genel Bakış Panosu][5]
   
    2. Üzerinde **İzleyici** sekmesinde daha derinlikli bir bakış için diğer birçok platforma özgü ölçümleri ekleyebilirsiniz. Bildirim hub'ları service için anında iletilen bildirim servisi Bildirim göndermeye çalıştığında döndürülen anında iletme bildirimi hizmeti ile ilgili özellikle hataları bakabilirsiniz. 
   
        ![Azure portal etkinlik günlüğü][6]
   
    3. Begin gözden geçirerek **gelen iletileri**, **kayıt işlemleri**, ve **başarılı bildirimler**. Ardından, anında iletme bildirimi hizmete özgü hatalarını gözden geçirmek için platform başına sekmesine gidin. 
   
    4. Bildirim hub'ınız için kimlik doğrulama ayarlarını hatalıysa, ileti **PNS kimlik doğrulama hatası** görüntülenir. Bu, anında bildirim hizmeti kimlik bilgileri denetlemek için iyi bir göstergesidir. 

* **Programlı erişim**

    Programlı erişim hakkında daha fazla bilgi için bu makalelere bakın: 

    * [Programsal telemetri erişim]  
    * [API örnek üzerinden telemetri erişim] 

    > [!NOTE]
    > Telemetri ile ilgili çeşitli özellikler verme ister ve kayıtlar ve telemetri erişim API'leri aracılığıyla alma standart hizmet katmanında kullanılabilir. Kullanmayı denerseniz, bu özellikler ücretsiz veya temel hizmet katmanını, REST API'leri özelliklerinden doğrudan kullanırsanız, SDK'sı ve HTTP 403 (Yasak) hata kullanırsanız bir özel durum iletisi alırsınız. 
    >
    >Telemetri ile ilgili özellikleri kullanmak için önce standart hizmet katmanında kullandığınız Azure portalında sağlayın.  
> 
> 

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
[şablonları]: https://msdn.microsoft.com/library/dn530748.aspx 
[APNs genel bakış]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html
[hakkında FCM iletileri]: https://firebase.google.com/docs/cloud-messaging/concept-options
[Export and modify registrations in bulk]: http://msdn.microsoft.com/library/dn790624.aspx
[hizmet veri yolu Gezgini]: https://msdn.microsoft.com/library/dn530751.aspx#sb_explorer
[hizmet veri yolu Gezgini kod]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Bildirim hub'ları için cihaz kayıtları görüntüle]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[Derin Dalış: Visual Studio 2013 güncelleştirme 2 RC ve Azure SDK 2.3]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[Visual Studio 2013 güncelleştirme 3'ü ve Azure SDK 2.4 sürümü Duyurusu]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend]: https://docs.microsoft.com/dotnet/api/microsoft.azure.notificationhubs.notificationhubclient.enabletestsend?view=azure-dotnet
[Programsal telemetri erişim]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[API örnek üzerinden telemetri erişim]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

