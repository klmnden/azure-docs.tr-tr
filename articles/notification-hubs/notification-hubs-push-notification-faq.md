---
title: "Azure bildirim hub'ları: Sık sorulan sorular (SSS) | Microsoft Docs"
description: "Bildirim hub'ları üzerinde çözümleri tasarlama/uygulama konusunda sık sorulan sorular"
services: notification-hubs
documentationcenter: mobile
author: ysxu
manager: erikre
keywords: "anında iletme bildirimi, anında iletme bildirimleri, iOS anında iletme bildirimleri, android anında iletme bildirimleri, ios anında iletme, android anında iletme"
editor: 
ms.assetid: 7b385713-ef3b-4f01-8b1f-ffe3690bbd40
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 01/19/2017
ms.author: yuaxu
ms.openlocfilehash: d19a1b7c8d50ef0fde3cf65c9fd469bc34a27adc
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="push-notifications-with-azure-notification-hubs-frequently-asked-questions"></a>Anında iletme bildirimleri ile Azure Notification Hubs: sık sorulan sorular
## <a name="general"></a>Genel
### <a name="what-is-the-resource-structure-of-notification-hubs"></a>Bildirim hub'ları kaynak yapısını nedir?

Azure bildirim hub'ları sahip iki kaynak düzey: hub'ları ve ad alanları. Bir hub'ı bir uygulama platformlar arası anında bilgileri içerebilen bir tek itme kaynaktır. Bir ad alanı, hub'ları bir bölgede koleksiyonudur.

Önerilen eşleme bir uygulama içeren bir ad ile eşleşir. Ad alanı içinde üretim uygulamanızın üzerinde çalıştığı bir üretim hub, sınama uygulama vb. ile çalışan test hub'ı sahip olabilir.

### <a name="what-is-the-price-model-for-notification-hubs"></a>Bildirim hub'ları için fiyat modeli nedir?
Son fiyatlandırma ayrıntıları bulunabilir [bildirim hub'ları fiyatlandırması] sayfası. Bildirim hub'ları ad alanı düzeyinde faturalandırılır. (Bir ad alanı tanımını için "Bildirim hub'ları kaynak yapısını nedir?" konusuna bakın) Bildirim hub'ları üç sunar:

* **Ücretsiz**: Bu katmanı gönderme özellikleri keşfetmek için iyi bir başlangıç noktası olduğu. Üretim uygulamaları için önerilmez. 500 aygıtları alın ve hizmet düzeyi sözleşmesi (SLA) garanti ile 1 milyondan fazla ad alanı, ayda başına dahil edilen iter.
* **Temel**: Bu katmanda (veya standart katmanı) küçük üretim uygulamaları için önerilir. 200.000 aygıtları alın ve 10 milyon başına aylık başlangıç noktası olarak ad alanı dahil iter. Kota büyüme seçenekleri dahil edilir.
* **Standart**: Bu katmanı Orta büyük üretim uygulamaları için önerilir. 10 milyon aygıtları alın ve 10 milyon başına aylık başlangıç noktası olarak ad alanı dahil iter. Kota artırma seçenekleri ve zengin telemetri yetenekleri dahil edilir.

Standart katman özellikleri:
* **Zengin telemetri**: hata ayıklama için gönderme istekleri ve Platform bildirim sistemi geri bildirim izlemek için ileti Telemetri başına bildirim hub'ları kullanabilirsiniz.
* **Çoklu müşteri mimarisi**: bir ad alanı düzeyinde Platform bildirim sistem kimlik bilgileri ile çalışabilirsiniz. Bu seçenek, aynı ad içinde hub'ları kolayca kiracılar Böl olanak tanır.
* **Zamanlanmış itme**: bildirimleri dilediğiniz zaman gönderilmek üzere zamanlayabilirsiniz.

### <a name="what-is-the-notification-hubs-sla"></a>Bildirim hub'ları SLA nedir?
Temel ve standart bildirim hub'ları katmanları için düzgün şekilde yapılandırılan uygulamaların anında iletme bildirimleri göndermek veya kayıt yönetimi en az zaman yüzde 99,9 işlemleri. SLA hakkında daha fazla bilgi için şuraya gidin [bildirim hub'ları SLA](https://azure.microsoft.com/support/legal/sla/notification-hubs/) sayfası.

> [!NOTE]
> Anında iletme bildirimleri üçüncü taraf Platform bildirim sistemleri (örneğin, Apple APNS ve Google FCM) bağımlı olduğundan, bu iletiler teslim için SLA garantisi yoktur. Bu bildirim hub'ları toplu Platform bildirim sistemleri (SLA) garanti gönderdikten sonra gönderim (SLA garanti) sunmak için Platform bildirim sistemleri sorumluluğundadır.

### <a name="which-customers-are-using-notification-hubs"></a>Hangi müşterilerin bildirim hub'ları kullanıyor musunuz?
Birçok müşteri bildirim hub'ları kullanın. Bazı önemli olanları aşağıda listelenmiştir:

* Sochi 2014: İlgi alanı gruplarına, 3 + milyon aygıtları ve iki hafta içinde gönderilen 150 + milyon bildirimler yüzlerce. [Örnek olay incelemesi: Sochi]
* Skanska: [örnek olay incelemesi: Skanska]
* Seattle süresi: [örnek olay incelemesi: Seattle saatleri]
* Mural.LY: [örnek olay incelemesi: Mural.ly]
* 7Digital: [örnek olay incelemesi: 7Digital]
* Bing uygulamalar: Cihazları milyonlarca günde 3 milyon bildirimleri gönderin.

### <a name="how-do-i-upgrade-or-downgrade-my-hub-or-namespace-to-a-different-tier"></a>Nasıl yükseltme veya my hub veya ad alanı farklı bir katmana düşürmek?
Git  **[Azure portal]** > **bildirim hub'ları ad alanları** veya **bildirim hub'ları**. Güncelleştirme ve Git istediğiniz kaynağı seçin **fiyatlandırma katmanı**. Aşağıdaki gereksinimleri dikkate alın:

* Güncelleştirilmiş fiyatlandırma katmanı uygulandığı *tüm* hub'ları ile çalışırken ad alanında.
* Cihaz sayısı için olana katmanı sınırını aşarsa, aygıtlar, indirgeme önce silmeniz gerekir.


## <a name="design-and-development"></a>Tasarım ve geliştirme
### <a name="which-server-side-platforms-do-you-support"></a>Sunucu tarafı platformları destekliyor?
Sunucu SDK'ları, .NET, Java, Node.js, PHP ve Python için kullanılabilir. Farklı platformlarda kullanıyorsanız veya ek bağımlılık istemediğiniz doğrudan REST API'leri ile çalışabilmek için bildirim hub'ları API'leri, REST arabirimleri temel alır. Daha fazla bilgi için Git [bildirim hub'ları REST API'leri] sayfası.

### <a name="which-client-platforms-do-you-support"></a>Hangi istemci platformlarını destekliyor?
Anında iletme bildirimleri için desteklenen [iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [Windows Evrensel](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [(aracılığıyla, Baidu) Android Çin](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) ve [Android](xamarin-notification-hubs-push-notifications-android-gcm.md)), [Chrome uygulamaları](notification-hubs-chrome-push-notifications-get-started.md), ve [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari). Daha fazla bilgi için Git [bildirim hub'ları başlama öğreticileri] sayfası.

### <a name="do-you-support-text-message-email-or-web-notifications"></a>SMS mesajı, e-posta veya web bildirimleri destekliyor musunuz?
Bildirim hub'ları öncelikle mobil uygulamalara bildirim göndermek için tasarlanmıştır. İleti özellikleri e-posta veya metin sağlamaz. Ancak, bu yetenekleri sağlamak üçüncü taraf platformları kullanarak yerel anında iletme bildirimleri göndermek için Notification Hubs ile tümleştirilebilir [Mobile Apps].

Bildirim hub'ları da sağlamaz bir tarayıcı içi anında bildirim teslim hizmeti sunuyoruz. Müşteriler, desteklenen sunucu tarafı platformlar üzerinde SignalR kullanarak bu özelliği uygulayabilirsiniz. Chrome korumalı alan tarayıcı uygulamalarında bildirimleri göndermek istiyorsanız, bkz: [Chrome uygulamaları Öğreticisi].

### <a name="how-are-mobile-apps-and-azure-notification-hubs-related-and-when-do-i-use-them"></a>Mobile Apps ve Azure Notification Hubs'ın ilgili şeklini ve ne zaman bunları kullanabilirim?
Geri varolan mobil uygulama varsa son ve anında iletme bildirimleri göndermek için yalnızca özelliği eklemek istiyorsanız, Azure bildirim hub'ları kullanabilirsiniz. Mobil uygulamanızı geri son sıfırdan ayarlamak istiyorsanız, Azure App Service Mobile Apps özelliğini kullanmayı düşünün. Böylece mobil uygulama arka uçtan anında iletme bildirimleri kolayca gönderebilir mobil uygulama bildirim hub'ı otomatik olarak sağlar. Mobile Apps için fiyatlandırma bir bildirim hub'ı temel ücretleri içerir. Dahil edilen iter aşan zaman yalnızca ücret ödersiniz. Maliyetleri hakkında daha fazla ayrıntı için Git [App Service fiyatlandırması] sayfası.

### <a name="how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>Notification Hubs ile anında iletme bildirimleri göndermek istediğinizde kaç cihaz t destekler?
Başvurmak [bildirim hub'ları fiyatlandırması] desteklenen aygıtların sayısı hakkında ayrıntılı bilgi için sayfa.

10 milyondan fazla kayıtlı cihazlar için destek gerekiyorsa [Bize Ulaşın](https://azure.microsoft.com/overview/contact-us/) doğrudan çözümünüzü ölçek yardımcı.

### <a name="how-many-push-notifications-can-i-send-out"></a>Kaç tane anında iletme bildirimleri için gönderebilirim?
Seçili katmanına bağlı olarak Azure bildirim hub'ları otomatik olarak sistem üzerinden akan bildirim sayısı temel alınarak ölçek artırabilir.

> [!NOTE]
> Genel kullanım maliyeti sunulmasını anında iletme bildirimleri sayısına göre artırabilir. Üzerindeki ana hatlarıyla katmanı sınırlarının haberdar olduğunuzdan emin olun [bildirim hub'ları fiyatlandırması] sayfası.
> 
> 

Müşterilerimizin her gün milyonlarca anında iletme bildirimi göndermek için Notification Hubs'ı kullanın. Azure Notification Hubs kullandığınız sürece, anında iletme bildirimleri ulaşabileceği ölçeklendirmek için özel bir şey yapmanız gerekmez.

### <a name="how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>Ne kadar gönderilen sürer anında iletme bildirimleri cihazımı ulaşması?
Burada gelen yük tutarlı ve hatta, Azure Notification Hubs en az işleyebilir normal kullanım senaryosunda, *1 milyon anında iletme bildirimi gönderir bir dakika*. Bu oran, etiket sayısı, gelen gönderir ve diğer dış faktörlere doğasına bağlı olarak değişebilir.

Tahmini teslimat zamanı sırasında hizmet platformu başına hedefleri hesaplar ve kayıtlı etiketleri veya etiket ifadeleri yollar iletileri için anında bildirim hizmeti (PNS) göre. Cihaza bildirimleri göndermek için PNS'ye sorumluluğundadır.

PNS bildirimleri teslim etmek için tüm SLA garanti etmez. Bununla birlikte, çoğu anında iletme bildirimleri hedef cihazlara (genellikle 10 dakika içinde) birkaç dakika içinde bildirim hub'ları için gönderilen zamandan teslim edilir. Birkaç bildirimleri daha fazla zaman alabilir.

> [!NOTE]
> Azure bildirim hub'ları için PNS 30 dakika içinde teslim olmayan herhangi bir anında bildirim bırakma yerinde olan bir ilkesi var. Bu gecikme nedeniyle, bir dizi için oluşabilir, ancak çoğu PNS, uygulamanızın yaygın olarak azaltma olduğundan.
> 
> 

### <a name="is-there-any-latency-guarantee"></a>Herhangi bir gecikme garanti var mı?
Anında iletme bildirimleri (bir dış, platforma özgü PNS tarafından teslim edilir) yapısı nedeniyle, gecikme garantisi yoktur. Genellikle, anında iletme bildirimleri çoğunluğu birkaç dakika içinde dağıtılır.

### <a name="what-do-i-need-to-consider-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>Ad alanları ve bildirim hub'ları ile bir çözüm tasarlarken göz önünde bulundurmanız gerekenler nelerdir?
#### <a name="mobile-appenvironment"></a>Mobil uygulama/ortamı
* Bir bildirim hub'ı ortamı başına mobil uygulama başına kullanın.
* Çok kiracılı bir senaryoda, her bir kiracı ayrı bir hub'ı olmalıdır.
* Hiçbir zaman üretim için aynı bildirim hub'ı paylaşma ve test ortamları. Bu yöntem, bildirimleri gönderirken sorunlara neden olabilir. (Korumalı alan ve üretim gönderme uç noktalar, her biri ayrı kimlik bilgilerine sahip Apple sunar.)
* Varsayılan olarak, Azure portalı üzerinden veya Visual Studio Azure tümleşik bileşeni, kayıtlı cihazlara test bildirimleri gönderebilirsiniz. Kayıt havuzundan rastgele seçilir 10 cihazlara eşiğini ayarlayın.

> [!NOTE]
> Hub'ınızı başlangıçta bir Apple sandbox sertifikayla yapılandırılmış ve ardından bir Apple üretim sertifikası kullanacak şekilde yeniden, özgün cihaz belirteçleri geçersizdir. Geçersiz belirteçleri Gönderim başarısız olmasına neden. Üretim ve test ortamlarınızı ayırmak ve farklı ortamlar için farklı hub'ları kullanın.
> 
> 

#### <a name="pns-credentials"></a>PNS kimlik bilgileri
Uygulama tanımlayıcısı ve güvenlik belirteçleri (örneğin, Apple veya Google) bir platformun Geliştirici Portalı ile mobil uygulama kaydedildikten sonra gönderilir. Uygulama arka uç platformun PNS bu belirteçleri sunar, böylece cihazlara anında iletme bildirimleri gönderilebilir. Güvenlik belirteçleri, sertifikaları (örneğin, Apple iOS veya Windows Phone) veya güvenlik anahtarları (örneğin, Google Android veya Windows) biçiminde olabilir. Bildirim hub'ları yapılandırılmalıdır. Yapılandırma, tipik olarak bildirim hub'ı düzeyinde yapılır, ancak çok müşterili bir senaryoda ad alanı düzeyinde de yapılabilir.

#### <a name="namespaces"></a>Ad Alanları
Ad alanları dağıtım gruplandırma için kullanılabilir. Çok kiracılı bir senaryoda aynı uygulamanın tüm kiracılar için tüm bildirim hub'ları temsil etmek için de kullanılabilir.

#### <a name="geo-distribution"></a>Coğrafi dağılımı
Coğrafi dağıtım her zaman anında iletme bildirimi senaryolarda önemli değildir. Cihazlara anında iletme bildirimleri teslim çeşitli PNSes (örneğin, APNS veya GCM) dağılımla değil.

Genel olarak kullanılan bir uygulamanız varsa, dünyanın dört farklı Azure bölgelerinde bildirim hub'ları hizmetini kullanarak, hub'ları farklı ad alanlarında oluşturabilirsiniz.

> [!NOTE]
> Özellikle kayıtlar için Yönetim maliyetini artırır Biz bu düzenlenmesi önerilmez. Yalnızca açık bir gereksinimi varsa yapılmalıdır.
> 
> 

### <a name="should-i-do-registrations-from-the-app-back-end-or-directly-through-client-devices"></a>Kayıtlar uygulama arka uçtan veya doğrudan istemci cihazları yapmalıyım?
Uygulama arka uçtan kayıtlar kayıt oluşturmadan önce istemcilerin kimliğini doğrulamak olduğunda faydalıdır. Oluşturulan veya uygulama mantığına göre uygulama arka ucu tarafından değiştirilen etiketleri olduğunda da yararlı oldukları. Daha fazla bilgi için Git [arka uç kaydı Kılavuzu] ve [arka uç kaydı Kılavuzu 2] sayfaları.

### <a name="what-is-the-push-notification-delivery-security-model"></a>Anında iletme bildirimi teslimi güvenlik modeli nedir?
Azure bildirim hub'ları kullanan bir [paylaşılan erişim imzası](../storage/common/storage-dotnet-shared-access-signature-part-1.md)-tabanlı güvenlik modeli. Kök ad alanı düzeyinde veya ayrıntılı bildirim hub'ı düzeyi, paylaşılan erişim imzası belirteçleri kullanabilirsiniz. Paylaşılan erişim imzası belirteçleri farklı yetkilendirme kuralları, örneğin izlemek için ileti izinleri göndermek için veya bildirim izinlerini dinlemek için ayarlanabilir. Daha fazla bilgi için bkz: [bildirim hub'ları güvenlik modeli] belge.

### <a name="how-should-i-handle-sensitive-payload-in-push-notifications"></a>Anında iletme bildirimleri hassas yükünde nasıl işleneceğini?
Tüm bildirimler platformun PNS tarafından hedef aygıtlara teslim edilir. Bir bildirim için Azure Notification Hubs gönderildiğinde, işlenen ve ilgili PNS geçirildi.

Azure Notification Hubs ' PNS gönderene gelen tüm bağlantıları HTTPS kullanın.

> [!NOTE]
> Azure bildirim hub'ları iletileri yükü herhangi bir şekilde günlüğe kaydetmez.
> 
> 

Hassas yüklerini göndermek için güvenli itme düzeni kullanmanızı öneririz. Gönderenin hassas yükü olmadan cihaz İleti tanımlayıcısı içeren bir ping bildirim sağlar. Cihaza uygulamanın yükü aldığında, uygulama ileti ayrıntılarını doğrudan getirmek için güvenli bir API çağırır. Bu desen uygulamak nasıl bir kılavuz için Git [güvenli bildirim hub'ları itme öğretici] sayfası.

## <a name="operations"></a>İşlemler
### <a name="what-support-is-provided-for-disaster-recovery"></a>Olağanüstü durum kurtarma için hangi destek sağlanır?
Bizden (bildirim hub'ları adı, bağlantı dizesi ve diğer kritik bilgileri) meta veri olağanüstü durum kurtarma kapsamı sunuyoruz. Bir olağanüstü durum kurtarma senaryosunda tetiklendiğinde kayıt verilerdir *yalnızca segmentlere* kaybolur bildirim hub'ları altyapısının. Bu veriler, yeni hub kurtarma sonrası dışındaki bir çözümü uygulamak gerekir:

1. Bir ikincil bildirimler hub'ı farklı bir veri merkezinde oluşturun. Bir yönetim özelliklerinizi etkileyebilecek bir olağanüstü durum kurtarma olayı korumak için en baştan oluşturmanızı öneririz. Bir olağanüstü durum kurtarma olayı defada de oluşturabilirsiniz.

2. İkincil bildirim hub'ı birincil bildirim hub'ınızı kayıtları ile doldurun. İki hub kayıtları korumak ve kayıtlar gelen eşit tutmak çalışırken öneririz yok. Bu yöntem PNS tarafında süresi dolacak şekilde devralınmış eğilimi iyi kayıtların nedeniyle çalışmıyor. Bildirim hub'ları temizler bunları süresi dolmuş veya geçersiz kayıtlar hakkında PNS geri bildirim aldığı gibi.  

Uygulama arka uçları için iki önerileri sunuyoruz:

* Kayıtlar kendi uçta belirli bir dizi tutan bir uygulama arka uç kullanın. İkincil bildirim hub'ına toplu ekleme daha sonra gerçekleştirebilirsiniz.

* Yedek olarak birincil bildirim hub'ı kayıtlar normal bir dökümünü alır bir uygulama arka ucu kullanın. İkincil bildirim hub'ına toplu ekleme daha sonra gerçekleştirebilirsiniz.

> [!NOTE]
> Kayıtlar içeri/dışarı aktarma işlevleri standart katmanda kullanılabilir açıklanan [kayıtlar dışa aktarma/içe aktarma] belge.
> 
> 

Hedef cihazlarda uygulama başlatıldığında bir arka uç yoksa, ikincil bildirim hub ' yeni bir kayıt gerçekleştirin. Sonuçta ikincil bildirim hub'ı kayıtlı tüm etkin aygıtlar sahip olur.

Açılmamış uygulamalar içeren cihazları bildirimleri zaman almazsınız bir zaman dilimi olacaktır.

### <a name="is-there-audit-log-capability"></a>Denetim günlüğü özelliği var mı?
Sunulan işlem günlükleri tüm bildirim hub'ları yönetim işlemlerinin Git [Azure portal].

## <a name="monitoring-and-troubleshooting"></a>İzleme ve sorun giderme
### <a name="what-troubleshooting-capabilities-are-available"></a>Sorun giderme hangi özellikler sağlanıyor?
Azure bildirim hub'ları, özellikle en yaygın senaryo bırakılan bildirimler için sorun giderme için birçok özellik sağlar. Ayrıntılar için bkz [bildirim hub'ları sorun giderme] teknik incelemesi.

### <a name="what-telemetry-features-are-available"></a>Hangi telemetri özellikler sağlanıyor?
Telemetri verileri görüntüleme azure Notification Hubs etkinleştirir [Azure portal]. Ölçümleri ayrıntılarını bulunur [bildirim hub'ları ölçümleri] sayfası.

> [!NOTE]
> Başarılı bildirimler yalnızca anında iletme bildirimleri (örneğin, APNS için Apple) veya GCM için Google dış PNS için teslim edilmediği anlamına gelir. Hedef aygıtlara bildirimleri göndermeyi PNS sorumluluğundadır. Genellikle, PNS teslim ölçümleri üçüncü taraflara kullanıma sunmuyor.  
> 
> 

Telemetri verileri programlı olarak (standart katman) verme özelliği de sunuyoruz. Ayrıntılar için bkz [bildirim hub'ları ölçümleri örnek].

[Azure portal]: https://portal.azure.com
[bildirim hub'ları fiyatlandırması]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Notification Hubs SLA]: http://azure.microsoft.com/support/legal/sla/
[Örnek olay incelemesi: Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[Örnek olay incelemesi: Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[Örnek olay incelemesi: Seattle saatleri]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[Örnek olay incelemesi: Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[Örnek olay incelemesi: 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[bildirim hub'ları REST API'leri]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[bildirim hub'ları başlama öğreticileri]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Chrome uygulamaları Öğreticisi]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[arka uç kaydı Kılavuzu]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[arka uç kaydı Kılavuzu 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[bildirim hub'ları güvenlik modeli]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[güvenli bildirim hub'ları itme öğretici]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[bildirim hub'ları sorun giderme]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[bildirim hub'ları ölçümleri]: https://msdn.microsoft.com/library/dn458822.aspx
[bildirim hub'ları ölçümleri örnek]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[kayıtlar dışa aktarma/içe aktarma]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure portal]: https://portal.azure.com
[complete samples]: https://github.com/Azure/azure-notificationhubs-samples
[Mobile Apps]: https://azure.microsoft.com/services/app-service/mobile/
[App Service fiyatlandırması]: https://azure.microsoft.com/pricing/details/app-service/
