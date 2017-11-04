---
title: "Azure Notification Hubs - tanılama yönergeleri"
description: "Azure Notification Hubs ile ilgili genel sorunları tanılamak nasıl yönergeleri."
services: notification-hubs
documentationcenter: Mobile
author: ysxu
manager: erikre
editor: 
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 32e3a2e6f840afd865375a622cfae0d33ba65090
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-notification-hubs---diagnosis-guidelines"></a>Azure Notification Hubs - tanılama yönergeleri
## <a name="overview"></a>Genel Bakış
Azure Notification Hubs Müşterilerden aldığımız en sık kullanılan sorulardan biri olduğunu anlamak üzere neden bunlar kendi uygulama arka ucundan gönderilen bir bildirim görmüyorum nasıl görünür nerede ve neden bildirimleri bırakılan istemci aygıtı - ve bu sorunu gidermek nasıl. Bu makalede aracılığıyla neden bildirimleri bırakılan veya bitmeyen çeşitli nedenlerle aygıtlarda ele alacağız. Biz çözümlemek ve kök nedenini anlamak yolu üzerinden arar. 

Öncelikle, Azure Notification Hubs bildirimleri aygıtlara nasıl iter anlaşılması önemlidir.
![][0]

Tipik gönderme bildirim akışı ileti gönderilen **uygulama arka uç** için **Azure bildirim hub'ı (NH)** hangi sırayla mu yapılandırılmış etiketleri & "hedefler" belirlemek için etiket ifadeleri dikkate alarak tüm kayıtlar üzerindeki bazı işleme anında iletme bildirimi alması gereken yani tüm kayıtlar. Bu kayıtlar herhangi birini veya tümünü bizim desteklenen platformlar - iOS, Google, Windows, Windows Phone yayılabilir Kindle ve Baidu Çin Android için. Hedefleri kurulan sonra NH sonra gönderim bildirimlerini çıkışı bölme belirli cihaz platformu için kaydı birden çok toplu arasında **anında bildirim hizmeti (PNS)** -Örneğin, APNS için Apple, Google vb. için GCM. Bildirim Hub'ı Yapılandır sayfasında Klasik Azure portalı kümesindeki kimlik bilgilerine göre ilgili PNS ile NH doğrular. PNS sonra ilgili bildirimlere iletir **istemci cihazları**. Bu, son Bacak bildirim teslimi için PNS platformu ve cihazın arasında yer alır ve anında iletme bildirimleri göndermek için yol önerilen platformudur. Bu nedenle dört ana bileşen - sahibiz *istemci*, *uygulama arka uç*, *Azure bildirim hub'ları (NH)* ve *anında bildirim Hizmetleri (PNS)* ve bunları herhangi biri kesiliyor bildirimleri neden olabilir. Bu mimarisi hakkında daha fazla bilgi edinilebilir [Notification Hubs'a genel bakış].

Bildirimleri göndermeyi hatası ilk test/Hazırlama işlemi sırasında bir yapılandırma sorunu gösterebilen aşama veya burada tüm ya da bildirimlerin bazıları bırakılan daha derin bazı uygulama belirten veya desen sorunu ileti alma üretimde oluşabilir ortaya çıkabilir. Bölümünde, aşağıda ortak değişen bazıları, belirgin bulabilirsiniz nadir türü ve diğerlerinin değil kadar çok çeşitli bırakılan bildirimler senaryolar ele alacağız. 

## <a name="azure-notifications-hub-mis-configuration"></a>Azure bildirim hub'ı yanlış yapılandırma
Başarıyla ilgili PNS bildirimleri göndermek için geliştiricinin uygulama bağlamında kendi kimliğini doğrulamak Azure Notification Hubs gerekir. Bu bir geliştirici hesabını ilgili platformun (Google, Apple, Windows vb.) oluşturma ve bildirim hub'ları yapılandırma bölümü altında portalında yapılandırılması gereken kimlik bilgilerini nereden uygulamasına kaydetme geliştirici tarafından mümkün hale getirilir. Hiçbir bildirim üzerinden yapıyorsanız, doğru kimlik bilgilerini bunları kendi platform özel Geliştirici hesabı altında oluşturulan uygulama ile eşleşen bildirim hub'ı yapılandırıldığından emin olmak için ilk adım olmalıdır. Size bizim [Başlarken öğreticilerine alma] üzerinde bu işlemi adım adım bir biçimde gitmek yararlıdır. Bazı sık karşılaşılan hatalı yapılandırmalar şunlardır:

1. **Genel**
   
    bir) yapma bildirim hub adınızı (olmadan, yazım hatalarını) aynı olduğundan emin:
   
   * Burada istemciden kaydediyorsunuz, 
   * Burada arka ucundan bildirim gönderiyorsanız,  
   * PNS kimlik bilgileri yapılandırdığınız ve 
   * Yapılandırdığınız istemci ve arka uç SAS kimlik bilgileri. 
     
     b) yapma emin istemci ve uygulama arka uç doğru SAS yapılandırma dizelerini kullanıyorsanız. Altın kural, kullandığınız gerekir **DefaultListenSharedAccessSignature** istemcide ve **DefaultFullSharedAccessSignature** (NH bildirim gönderebilmesi için izin veren) uygulama arka uç üzerinde
2. **Apple anında iletilen bildirim servisi (APNS) yapılandırma**
   
    -Bir üretim için iki farklı hub korumalıdır ve test etmek için başka bir amaçla. Bu, sanal ortamda ayrı hub'ına kullanacaksanız sertifika ve ayrı hub'ına üretimde kullanmak zorunda kalacaklarını sertifika karşıya anlamına gelir. Satır aşağı bildirim hatasına neden olabilecek şekilde farklı türde sertifikaları aynı hub'ına yüklemeye çalışmayın. Kendinizi burada yanlışlıkla sertifika farklı türde aynı hub'ına karşıya yüklediğiniz bir konumda bulursanız, hub'ını silmek ve yeni başlatmak için önerilir. Bazı nedenlerden dolayı sonra en azından hub'ını silmek mümkün değilse, var olan tüm kayıtlar hub'dan silmeniz gerekir. 
3. **Google Cloud Messaging (GCM) yapılandırma** 
   
    bir) yapma emin, "Google bulut Mesajlaşma için Android" bulut projenizi altına etkinleştireceğiniz olduğunu. 
   
    ![][2]
   
    b) yapma emin GCM ile kimlik doğrulaması için kimlik bilgileri hangi NH elde kullanır ancak bir "sunucu anahtarı" oluşturun. 
   
    ![][3]
   
    c) yapma emin panodan edinebilirsiniz tamamen sayısal bir varlık istemcide "Proje kimliği" yapılandırmış olduğunuz:
   
    ![][1]

## <a name="application-issues"></a>Uygulama sorunları
1) **Etiketler / etiket ifadeleri**

Kullanarak hedef kitlenizi segmentlere etiketleri veya etiket ifadeleri kullanıyorsanız, her zaman bildirim gönderirken olduğunu gönderme aramanız belirten etiketler/etiket ifadeleri dayalı bulunan hiçbir hedef mümkündür. Bildirim göndermek ve sonra bu kayıtları olan istemcilerden yalnızca bildirim alındığını doğrulamak bağlandığınızda eşleşen etiketleri olduğundan emin olmak için kayıtları gözden geçirmek en iyisidir. Örneğin Tüm NH, kayıtlarla ile yapıldığını etiketi "Siyaset" söyleyin ve "Spor" etiketi içeren bir bildirim gönderiyorsanız, herhangi bir cihaza gönderilmez. Karmaşık bir durumda, size yalnızca kayıtlı "Etiketi A" veya "Etiketi B" ile ancak bildirimleri gönderilirken etiket ifadeleri ilgili olabilir "Etiketi A & & etiketi B" hedefleme. Kendi kendine tanılamak ipuçları aşağıdaki bölümde, sahip oldukları etiketlerle birlikte, kayıtları gözden geçirebilirsiniz yolu vardır. 

2) **Şablon sorunları**

Şablonları kullanarak sonra aşağıdaki olun yönergeleri bölümünde anlatıldığı [şablonu Kılavuzu]. 

3) **Geçersiz kayıtlar**

Bildirim hub'ı doğru şekilde yapılandırılmış ve doğru şekilde bildirimlerin gönderilmesi gereken geçerli hedefler bulunacak kaynaklanan tüm etiketleri/etiket ifadeleri kullanılan varsayıldığında, paralel - kayıtlar kümesine iletileri gönderen her toplu işlemin birkaç işleme toplu işlemi kapatmak NH ateşlenir. 

> [!NOTE]
> Biz paralel işleme yapmak olduğundan, biz bildirimleri teslim siparişin garanti etmez. 
> 
> 

Artık Azure bildirim hub'ı "konumundaki çoğu kez" ileti teslim modeli için optimize edilmiştir. Bu bildirim yok bir cihaza birden çok kez teslim edilir böylece biz devre dışı bırakma çoğaltma denemesi anlamına gelir. Bu emin olmak için kayıtlar arayın ve yalnızca tek bir iletiyi PNS gerçekten iletiyi göndermeden önce cihaz tanımlayıcısı gönderilen emin olun. Her toplu sırayla kabul etmek ve kayıtları doğrulanıyor, PNS için gönderilen PNS bir veya daha fazla bir toplu kayıtlar hatayla Azure NH hata verir ve böylece toplu tamamen bırakarak işlemeyi durdurur algılar mümkündür. Bu, özellikle bir TCP akışı protokolünü kullanan APNS ile geçerlidir. Bu teslim durumda sonra biz adresindeki-çoğu için iyileştirilmiş ancak bizim veritabanından hataya neden olan kaydı kaldırmak ve bildirim teslim toplu aygıtları geri kalanı için yeniden deneyin.

Azure Notification Hubs REST API'lerini kullanarak bir kayıt karşı başarısız teslim girişimi için hata bilgilerini elde edebilirsiniz: [başına ileti Telemetri: bildirim iletisi Telemetri almak](https://msdn.microsoft.com/library/azure/mt608135.aspx) ve [PNS geri bildirim](https://msdn.microsoft.com/library/azure/mt705560.aspx). Bkz: [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) örneğin kod.

## <a name="pns-issues"></a>PNS sorunları
Bildirim iletisi ilgili PNS tarafından alındığında sonra bu cihaza bildirim teslim etmek için kendi sorumluluğundadır. Azure bildirim hub'ları burada resim dışında ve denetimi olduğunda veya cihaza teslim edilecek bildirim edecekse yoktur. Platform Bildirim Hizmetleri oldukça sağlam olduğundan, bildirimler cihazlara birkaç saniye içinde PNS ulaşmak eğilimindedir. PNS ancak azaltma, Azure Notification Hubs bir üstel geri alma stratejisi sonra uygulanır ve PNS için 30 dakikalık ulaşılamaz kalırsa sonra biz bir ilke süresinin dolmasını ve bu iletileri kalıcı olarak bırakma kullanıyor. 

Bir bildirim teslim etmek bir PNS çalışır, ancak cihaz çevrimdışıysa, bildirim sınırlı bir süre için PNS tarafından depolanır ve kullanılabilir hale geldiğinde cihaza teslim. Belirli bir uygulama için yalnızca bir son bildirim depolanır. Cihaz çevrimdışı durumdayken birden fazla bildirim gönderiyorsanız, her bir yeni bildirim atılmak üzere önceki bildirim neden olur. Bu davranış yalnızca en yeni bildirim tutmayı APNS bildirimleri birleştirmesi ve (hangi çöken bir anahtar kullanan) GCM daraltma olarak adlandırılır. Cihaz uzun bir süredir çevrimdışı kalırsa, bunun için depolanmakta herhangi bir bildirim atılır. Kaynak - [APNS Kılavuzu] & [GCM Kılavuzu]

Azure Notification Hubs ile - genel kullanarak bir HTTP üstbilgisi birleştirmesi anahtar geçirebilirsiniz `SendNotification` API (.NET SDK – örneğin `SendNotificationAsync`) olan ilgili PNS, ayrıca olarak geçirilen HTTP üstbilgileri alır. 

## <a name="self-diagnose-tips"></a>Kendi kendine tanılamak ipuçları
Çeşitli burada inceleyeceğiz tanılamak ve kök yolu tüm bildirim hub'ı sorunlarına neden:

### <a name="verify-credentials"></a>Kimlik bilgilerini doğrulama
1. **PNS Geliştirici Portalı**
   
    İlgili PNS Geliştirici Portalı (APNS, GCM, WNS vb.) kullanarak doğrulama bizim [Başlarken öğreticilerine alma].
2. **Klasik Azure portalı**
   
    Gözden geçirmek ve PNS Geliştirici Portalı'ndan elde edilenlerle kimlik bilgileriyle eşleşmesi için yapılandırma sekmesine gidin. 
   
    ![][4]

### <a name="verify-registrations"></a>Kayıtlar doğrulayın
1. **Visual Studio**
   
    Ardından geliştirme için Visual Studio kullanıyorsanız, Microsoft Azure ve görünüm bağlanmak ve bildirimler hub'ının "Sunucu Gezgini'nden" dahil olmak üzere Azure hizmetlerinin bir demet yönetmek kullanabilirsiniz. Bu, geliştirme ve test ortamınız için özellikle yararlıdır. 
   
    ![][9]
   
    Görüntüleyin ve hangi platformu için yerel sorunsuz şekilde sınıflandırılır tüm kayıtlar hub'ınızdaki veya şablon kayıt, herhangi bir etiket, PNS tanımlayıcısı, kayıt kimliği ve sona erme tarihini yönetin. Bir kayıt deyin herhangi bir etiket düzenlemek istiyorsanız kullanışlıdır anında - da düzenleyebilirsiniz. 
   
    ![][8]
   
   > [!NOTE]
   > Kayıtları düzenleme için visual Studio işlevi yalnızca geliştirme ve test kayıtları sınırlı sayıda sırasında kullanılmalıdır. Toplu olarak, kayıtlar düzeltme gerek oluşursa dışa aktarma/içe aktarma kullanmayı burada - açıklanan kayıt işlevselliği [içeri/dışarı aktarma kayıtlar](https://msdn.microsoft.com/library/dn790624.aspx)
   > 
   > 
2. **Hizmet veri yolu Gezgini**
   
    Birçok müşteri explorer açıklanan ServiceBus burada - kullanmak [ServiceBus Explorer] görüntüleme ve kendi bildirim hub'ı yönetme. Açık kaynaklı proje code.microsoft.com - kullanılabilir olduğu [ServiceBus Explorer kodu]

### <a name="verify-message-notifications"></a>İleti bildirimlerini doğrulayın
1. **Klasik Azure Portalı**
   
    Herhangi bir hizmet arka uçtan yukarı gerek ve çalışan istemcilerinize test bildirimleri göndermek için "Hata ayıklama" sekmesini gidebilirsiniz. 
   
    ![][7]
2. **Visual Studio**
   
    Visual Studio comforts test bildirimleri gönderebilirsiniz:
   
    ![][10]
   
    Daha fazla bilgiyi burada - Visual Studio bildirim hub'ı Azure explorer işlevselliği hakkında 
   
   * [VS Sunucu Gezgini genel bakış]
   * [VS Sunucu Gezgini Blog Gönderisi - 1]
   * [VS Sunucu Gezgini Blog Gönderisi - 2]

### <a name="debug-failed-notifications-review-notification-outcome"></a>Hata ayıklama başarısız bildirimleri / bildirim sonucunu gözden geçirin
**EnableTestSend özelliği**

Bildirim hub'ları aracılığıyla bir bildirim gönderdiğinizde, başlangıçta, yalnızca tüm hedefleri bulmak için işlem yapmak NH için sıraya ve ardından sonunda NH PNS gönderir. Bu, REST API veya istemci SDK birini kullanırken, gönderme aramanız başarılı dönüş yalnızca iletiyi başarıyla bildirim Hub'sıraya alındı, anlamına anlamına gelir. PNS için ileti göndermek NH sonunda var olduğunda ne içine bir öngörü vermez. İstemci aygıtı bildiriminizin ulaşan değil, NH PNS için ileti teslim almaya çalıştığında, örneğin yükü boyutu PNS tarafından izin verilen sınırı aştı bir hata oluştu veya geçersiz vb. NH içinde yapılandırılmış kimlik bilgileridir olasılığı yoktur. PNS hataları bir anlayış almak için biz adlı bir özellik sunulmuştur [EnableTestSend özelliği]. Bu özellik, portal ya da Visual Studio istemcisi sınama iletileri gönderirken otomatik olarak etkinleştirilir ve bu nedenle, ayrıntılı hata ayıklama bilgileri görmenize olanak sağlar. Tüm istemci SDK'ları için sonunda eklenir ve bu örnek artık kullanılabilir olduğunda .NET SDK'ın alma API'leri kullanabilirsiniz. Bu REST çağrısı ile kullanmak için yalnızca "test" gönderme aramanız sonunda örneğin olarak adlandırılan bir sorgu dizesi değişkeni ekleme 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Örnek (.NET SDK)*

Yerel bir bildirim göndermek için .NET SDK kullanarak varsayın:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);

`result.State`olur yalnızca durum `Enqueued` , anında iletme ne içine herhangi Insight olmadan yürütme işleminin sonunda. Kullanabileceğiniz artık `EnableTestSend` başlatılırken boolean özelliği `NotificationHubClient` ve bildirimi gönderirken hatalarla karşılaştı PNS hatalarla ilgili ayrıntılı durum elde edebilirsiniz. Buraya gönderme çağrısı NH bildirim sonucunu belirlemek için PNS için teslim edilen sonra yalnızca döndürmektir çünkü döndürmek için ek zaman alır. 

    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);

    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);

    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Örnek çıktı*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong

Bu ileti, bildirim hub'ı ya da hub'ında kayıtlar ile ilgili bir sorun ya da geçersiz kimlik bilgileri yapılandırılır ve bu kayıt silme ve iletiyi göndermeden önce yeniden istemci izin vermek için önerilen indirmelere olacaktır gösterir. 

> [!NOTE]
> Bu özellik kullanımı yoğun bir şekilde kısıtlanan ve bu nedenle, yalnızca bu kayıtlar sınırlı sayıda ile geliştirme ve test ortamında kullanmalısınız unutmayın. Biz yalnızca hata ayıklama bildirimlerini 10 cihazlara gönderme. Biz de dakikada 10 olması için hata ayıklama gönderir işleme sınırlaması vardır. 
> 
> 

### <a name="review-telemetry"></a>Telemetri gözden geçirin
1. **Klasik Azure portalını kullanın**
   
    Portal, bildirim hub'ına tüm etkinliklerin hızlı bir genel bakış almanızı sağlar. 
   
    a) "Pano" sekmesinden kayıtlar, bildirimler yanı sıra platform başına hata birleşik bir görünümünü görüntüleyebilirsiniz. 
   
    ![][5]
   
    b) da birçok başka platform belirli ölçümleri NH PNS Bildirim göndermeye çalıştığında döndürülen özellikle PNS belirli hataları daha derin bakmak için "İzleme" sekmesinden ekleyebilirsiniz. 
   
    ![][6]
   
    c), inceleyerek başlamalısınız **gelen iletileri**, **kayıt işlemleri**, **başarılı bildirimler** ve ardından her platform sekmesini PNS belirli hataları gözden geçirin. 
   
    d) ile kimlik doğrulama ayarları yanlış bildirim hub'ı varsa, PNS kimlik doğrulama hata iletisiyle karşılaşırsınız. PNS kimlik bilgilerini denetlemek için iyi bir göstergesidir. 

2) **Programlı erişim**

Daha fazla ayrıntı burada- 

* [Programsal Telemetri erişim]
* [API örnek üzerinden telemetri erişim] 

> [!NOTE]
> İlgili birkaç telemetri gibi özellikler **içeri/dışarı aktarma kayıtlar**, **API'leri aracılığıyla Telemetri erişim** vb. standart katmanında kullanılabilen yalnızca. Ücretsiz veya temel katmana varsa bu özellikleri kullanmayı denerseniz, bunları doğrudan REST API'lerinin kullanırken SDK ve HTTP 403 (Yasak) kullanırken bu etkili olması için özel durum iletisi alırsınız. Taşıdığınızdan emin olun kadar standart katmanı Klasik Azure portalı.  
> 
> 

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png

<!-- LINKS -->
[Notification Hubs'a genel bakış]: notification-hubs-push-notification-overview.md
[Başlarken öğreticilerine alma]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[şablonu Kılavuzu]: https://msdn.microsoft.com/library/dn530748.aspx 
[APNS Kılavuzu]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[GCM Kılavuzu]: http://developer.android.com/google/gcm/adv.html
[Export/Import Registrations]: http://msdn.microsoft.com/library/dn790624.aspx
[ServiceBus Explorer]: http://msdn.microsoft.com/library/dn530751.aspx
[ServiceBus Explorer kodu]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[VS Sunucu Gezgini genel bakış]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[VS Sunucu Gezgini Blog Gönderisi - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[VS Sunucu Gezgini Blog Gönderisi - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend özelliği]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Programsal Telemetri erişim]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[API örnek üzerinden telemetri erişim]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

