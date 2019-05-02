---
title: "Azure Notification hubs'ı: Sık sorulan sorular (SSS) | Microsoft Docs"
description: Bildirim hub'ları çözümleri tasarlama/uygulama konusunda sık sorulan sorular
services: notification-hubs
documentationcenter: mobile
author: jwargo
manager: patniko
editor: spelluru
keywords: anında iletme bildirimi, anında iletme bildirimleri, anında iletme bildirimlerini iOS, android anında iletme bildirimleri, ios anında iletme, android anında iletme
ms.assetid: 7b385713-ef3b-4f01-8b1f-ffe3690bbd40
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 03/11/2019
ms.author: jowargo
ms.openlocfilehash: 8af545f5700e90303562174a3c27cc5438b28e24
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925875"
---
# <a name="push-notifications-with-azure-notification-hubs-frequently-asked-questions"></a>Azure Notification Hubs ile anında iletme bildirimleri: Sık sorulan sorular

## <a name="general"></a>Genel

### <a name="what-is-the-resource-structure-of-notification-hubs"></a>Notification Hubs'ın kaynak yapısı nedir?

Azure Notification hubs'ı iki kaynak düzeyi vardır: hub'ları ve ad alanları. Bir hub'ı tek bir uygulama platformlar arası anında iletme bilgileri tutabilen bir tek bir anında iletme kaynaktır. Bir ad alanı, hub'ları bir bölgedeki bir koleksiyonudur.

Önerilen eşleme, bir ad alanı tek bir uygulama ile eşleşir. Ad alanı içinde üretim uygulamanızla birlikte çalışan bir üretim hub, kendi test uygulama vb. ile çalışan test hub'ı sahip olabilir.

### <a name="what-is-the-price-model-for-notification-hubs"></a>Bildirim hub'ları için fiyat modeli nedir?

En son fiyatlandırma ayrıntıları bulunabilir [Notification Hubs fiyatlandırması] sayfası. Notification hubs'ı ad alanı düzeyinde faturalandırılır. (Bir ad alanı tanımı için bkz: "Notification Hubs'ın kaynak yapısı nedir?") Bildirim hub'ları üç katmanda sunulur:

* **Ücretsiz**: Anında iletme bildirimleri keşfetmek için iyi bir başlangıç noktası katmandır. Üretim uygulamaları için önerilmez. 500 CİHAZDAN alın ve hizmet düzeyi sözleşmesi (SLA) garanti 1 milyon / ay, ad alanı başına dahil edilen gönderim.
* **Temel**: Bu katman (veya standart katman) küçük üretim uygulamaları için önerilir. 200.000 aygıt alma ve her ay temel olarak ad alanı başına dahil edilen 10 milyon bildirim gönderilir.
* **Standart**: Bu katman, Orta ve büyük üretim uygulamaları için önerilir. 10 milyon cihazı alın ve her ay temel olarak ad alanı başına dahil edilen 10 milyon bildirim gönderilir. Zengin telemetri (sağlanan itme durumuyla ilgili ek veriler) içerir.

Standart katman özellikleri:

* **Zengin telemetri**: Bildirim hub'ları ileti başına Telemetri, hata ayıklama için herhangi bir Platform bildirim sistemi geri bildirimi ve anında iletme istekleri izlemek için kullanabilirsiniz.
* **Çok kiracılı mimari**: Platform bildirim sistemi kimlik bilgileriyle bir ad alanı düzeyinde çalışabilir. Bu seçenek, aynı ad alanı kolayca kiracılar Bölünecek sağlar.
* **Zamanlanan anında iletme**: Bildirimleri dilediğiniz zaman gönderilmek üzere zamanlayabilirsiniz.
* **Toplu işlem**: Bölümünde anlatıldığı gibi kayıtlarını içeri/dışarı aktarma işlevi etkinleştirir [Kayıtları dışarı/içeri aktarma] belge.

### <a name="what-is-the-notification-hubs-sla"></a>Bildirim hub'ları SLA'sı nedir?

Temel ve standart Notification hub katmanları için düzgün şekilde yapılandırılmış uygulamaların anında iletme bildirimleri göndermek veya ilişkili kayıt yönetim işlemleri en az yüzde 99,9 oranında gerçekleştirin. SLA hakkında daha fazla bilgi için Git [bildirim hub'ları SLA](https://azure.microsoft.com/support/legal/sla/notification-hubs/) sayfası.

> [!NOTE]
> Üçüncü taraf Platform bildirim sistemlerinin (örneğin, Apple APNS ve Google FCM) anında iletme bildirimleri bağımlı olduğundan bu iletiler teslim için SLA garantisi yoktur. Bildirim hub'ları, Platform bildirim sistemlerinin (garantili SLA) için toplu işlerin gönderdikten sonra bildirim (SLA garantisi) sunmak için Platform bildirim sistemlerinin sorumluluğundadır.

### <a name="how-do-i-upgrade-or-downgrade-my-hub-or-namespace-to-a-different-tier"></a>Nasıl yükseltebilir veya my hub veya ad alanı farklı bir katmana düşürme?

Git  **[Azure portal]** > **Notification Hubs ad alanlarını** veya **Notification hubs'ı**. Güncelleştirme ve şuraya gitmek istediğiniz kaynağı seçin **fiyatlandırma katmanı**. Aşağıdaki gereksinimleri göz önünde bulundurun:

* Güncel fiyatlandırma katmanını uygulandığı *tüm* kullandığınız ad alanı, hub'ları.
* Cihaz sayısı indirgeme katmanı sınırını aşarsa, düşürme önce cihazları Sil gerekir.

## <a name="design-and-development"></a>Tasarım ve geliştirme

### <a name="which-server-side-platforms-do-you-support"></a>Sunucu tarafı hangi platformları destekliyor?

Sunucu SDK'ları, .NET, Java, Node.js, PHP ve Python için kullanılabilir. Farklı platformlar kullanıyorsanız veya ek bağımlılık istemediğiniz doğrudan REST API ile çalışabilmek için bildirim hub'ları API REST arabirimleri üzerinde temel alır. Daha fazla bilgi için Git [Notification hubs'ı REST API'leri] sayfası.

### <a name="which-client-platforms-do-you-support"></a>Hangi istemci platformlarını destekliyorsunuz?

Anında iletme bildirimleri için desteklenen [iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-fcm-get-started.md), [Windows Evrensel](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android Çin (Baidu) aracılığıyla](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) ve Android, [Chrome uygulamaları](notification-hubs-chrome-push-notifications-get-started.md), ve [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari). Daha fazla bilgi için Git [Notification hubs'ı kullanmaya başlama öğreticileri] sayfası.

### <a name="do-you-support-text-message-email-or-web-notifications"></a>SMS mesajı, e-posta veya web bildirimleri destekliyorsunuz?

Bildirim hub'ları öncelikli olarak bildirim göndermek için mobil uygulamalar için tasarlandı. İleti özellikleri e-posta veya metin sağlamaz. Ancak, bu yetenekleri sağlayan üçüncü taraf platformları kullanarak yerel anında iletme bildirimleri göndermek için Notification Hubs ile tümleştirilebilir [Mobile Apps].

Bildirim hub'ları da sağlamaz yepyeni bir tarayıcı içi anında iletme bildirimi teslim hizmeti. Müşteriler, üzerinde desteklenen sunucu tarafı platformları SignalR kullanarak bu özelliği uygulayabilir. Korumalı alan Chrome tarayıcı uygulamalarında bildirimleri göndermek istiyorsanız, bkz. [Chrome uygulama Öğreticisi].

### <a name="how-are-mobile-apps-and-azure-notification-hubs-related-and-when-do-i-use-them"></a>Mobile Apps ve Azure Notification Hubs'ın ilgili şeklini ve bunları ne zaman kullanırım?

Var olan bir mobil uygulama arka ucu varsa ve anında iletme bildirimleri göndermek için yalnızca özelliği eklemek istiyorsanız, Azure Notification hubs'ı kullanabilirsiniz. Sıfırdan, mobil uygulama arka ucu ayarlama istediğiniz Azure App Service Mobile Apps özelliğini kullanmayı düşünün. Mobil uygulama arka ucundan kolayca anında iletme bildirimleri gönderebilirsiniz, böylece bir mobil uygulama bir bildirim hub'ı otomatik olarak sağlar. Mobile Apps için fiyat, bildirim hub'ı temel ücretleri içerir. Dahil edilen gönderim uzun zaman yalnızca ödeme yaparsınız. Maliyetler hakkında daha fazla ayrıntı için Git [App Service fiyatlandırması] sayfası.

### <a name="how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>Kaç cihaz, Notification Hubs ile anında bildirimler gönderebilir miyim destekleyebilir mi?

Başvurmak [Notification Hubs fiyatlandırması] desteklenen aygıtların sayısı hakkında daha fazla ayrıntı için.

10 milyondan fazla kayıtlı cihazlar için desteğe ihtiyacınız varsa [bizimle](https://azure.microsoft.com/overview/contact-us/) doğrudan ve çözümünüzü ölçeklendirin yardımcı olacağız.

### <a name="how-many-push-notifications-can-i-send-out"></a>Kaç tane anında iletme bildirimleri için gönderebilirim?

Seçili katmanına bağlı olarak, Azure Notification hubs'ı otomatik olarak sistem üzerinden akıyor bildirim sayısı temel alınarak ölçeklendirilebilir.

> [!NOTE]
> Genel kullanım maliyeti sunulmasını anında iletme bildirimleri sayısına göre artırabilirsiniz. Şirket özetlenen katmanı sınırlarını haberdar olduğunuzdan emin olun [Notification Hubs fiyatlandırması] sayfası.

Müşterilerimiz, her gün milyonlarca anında iletme bildirimleri göndermek için Notification hubs'ı kullanın. Azure Notification hubs'ı kullanmakta olduğunuz sürece, anında iletme bildirimleri ulaşabileceği ölçeklendirmek için özel bir şey yapmanız gerekmez.

### <a name="how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>Ne kadar gönderilen sürer anında iletme bildirimleri cihazımı ulaşmak için?

Normal kullanım senaryosunda, burada gelen yükü tutarlıdır ve hatta, Azure Notification hubs'ı en az işleyebilir *1 milyon anında iletme bildirimi gönderen bir dakika*. Bu oran, etiket sayısı, gelen gönderir ve diğer dış faktörleri doğasına bağlı olarak değişebilir.

Tahmini teslimat süresini sırasında hizmet platformu başına hedefleri hesaplar ve kayıtlı etiketleri veya etiket ifadeleri yollar iletiler için anında iletme bildirimi hizmeti (PNS) göre. Cihaz için bildirimleri göndermek için PNS sorumluluğundadır.

PNS bildirimleri teslim etmek için herhangi bir SLA garanti etmez. Ancak, çoğu anında iletme bildirimleri hedef cihazlara (genellikle 10 dakika içinde) birkaç dakika içinde Notification hubs'ı gönderilmeden zamandan teslim edilir. Birkaç bildirimleri daha uzun sürebilir.

> [!NOTE]
> Azure Notification hubs'ı için PNS 30 dakika içinde teslim olmayan herhangi bir anında iletme bildirimleri doğrudan yerinde olan bir ilkesi var. Bu gecikme nedeniyle, bir dizi için oluşabilir ancak genellikle PNS uygulamanızı kısıtladığı çoğu.

### <a name="is-there-any-latency-guarantee"></a>Herhangi bir gecikme süresi garanti var mı?

Anında iletme bildirimleri (Dış, platforma özgü bir PNS tarafından sunulur) yapısı nedeniyle, gecikme süresi garantisi yoktur. Genellikle, anında iletme bildirimleri çoğunu birkaç dakika içinde dağıtılır.

### <a name="what-do-i-need-to-consider-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>Ad alanları ve notification hubs ile bir çözüm tasarlarken göz önüne almanız gereken ne gerekiyor?

#### <a name="mobile-appenvironment"></a>Mobil uygulama ortamı

* Mobil uygulama, her ortam başına bir bildirim hub'ı kullanın.
* Çok kiracılı bir senaryoda, her kiracıya ayrı bir hub'ı olması gerekir.
* Hiçbir zaman aynı bildirim hub'ı üretim için paylaşın ve test ortamları. Bu yöntem, bildirimleri gönderirken sorunlara neden olabilir. (Apple her ayrı kimlik bilgileriyle korumalı alan ve üretim anında iletme uç sunar.)
* Varsayılan olarak, kayıtlı cihazlarınızı Azure portal veya Azure tümleşik bileşen Visual Studio'da test bildirimleri gönderebilirsiniz. Kayıt havuzdan rastgele seçilmiş 10 cihazlara eşiği ayarlanır.

> [!NOTE]
> Hub'ınıza bir Apple korumalı alan sertifikayla yapılandırılmış ve ardından bir Apple üretim sertifikası kullanacak şekilde yapılandırıldı, özgün cihaz belirteçleri geçersizdir. Geçersiz belirteç bildirim başarısız olmasına neden. Üretim ve test ortamlarınızı ayırmak ve farklı ortamlar için farklı hub'ları kullanın.

#### <a name="pns-credentials"></a>PNS kimlik bilgileri

Bir mobil uygulama (örneğin, Apple veya Google) bir platformun developer portal ile kaydettiğinizde, uygulama tanımlayıcısı ve güvenlik belirteçleri gönderilir. Uygulama arka ucu, bu belirteçleri için platformun PNS sağlar, böylece cihazlara anında iletme bildirimleri gönderilebilir. Güvenlik belirteçleri, sertifikaları (örneğin, Apple iOS veya Windows Phone) veya güvenlik anahtarları (örneğin, Google Android veya Windows) biçiminde olabilir. Notification hub'ları yapılandırılmalıdır. Yapılandırma, genellikle bildirim hub'ı düzeyinde gerçekleştirilir, ancak bir çok kiracılı senaryosunda ad alanı düzeyinde de yapılabilir.

#### <a name="namespaces"></a>Ad Alanları

Ad alanları, dağıtım gruplandırma için kullanılabilir. Aynı uygulamanın bir çok kiracılı senaryosunda tüm kiracılar için tüm notification hubs'ı göstermek için de kullanılabilir.

#### <a name="geo-distribution"></a>Coğrafi dağıtım

Coğrafi dağıtım her zaman anında iletme bildirimi senaryolarda kritik değildir. Cihazlara anında iletme bildirimleri teslim çeşitli PNSes (örneğin, APNS veya FCM), eşit olarak dağıtılmış değildir.

Genel olarak kullanılan bir uygulamanız varsa, dünyanın dört bir yanındaki farklı Azure bölgelerindeki bildirim hub'ları hizmetini kullanarak farklı ad alanlarına hub'ları oluşturabilirsiniz.

> [!NOTE]
> Özellikle kayıtları için Yönetim maliyetini artırır çünkü bu düzenleme önerilmemektedir. Yalnızca açık bir gereksinimi varsa yapılması gerekir.

### <a name="should-i-do-registrations-from-the-app-backend-or-directly-through-client-devices"></a>Kayıtları bir uygulama arka ucundan veya doğrudan istemci cihazları yapmam gerekir?

Bir uygulama arka ucundan kayıtları, kayıt oluşturmadan önce istemcilerin kimliğini doğrulamak sahip olduğunda yararlıdır. Ayrıca kullanışlı oluşturulan veya uygulama mantığı temelinde uygulama arka ucu değiştiren etiketlere sahip olduğunda. Daha fazla bilgi için Git [Arka uç kayıt Kılavuzu] ve [arka uç kaydı Kılavuzu 2] sayfaları.

### <a name="what-is-the-push-notification-delivery-security-model"></a>Anında iletme bildirimi teslim güvenlik modeli nedir?

Azure Notification hubs'ı kullanan bir [paylaşılan erişim imzası](../storage/common/storage-dotnet-shared-access-signature-part-1.md)-tabanlı güvenlik modeli. Kök ad alanı düzeyinde veya ayrıntılı bildirim hub'ı düzeyinde paylaşılan erişim imzası belirteçlerine kullanabilirsiniz. Paylaşılan erişim imzası belirteçleri farklı yetkilendirme kuralları, örneğin izlemek için izinleri ileti göndermeye veya bildirim izinlerini dinleyecek şekilde ayarlanabilir. Daha fazla bilgi için [Bildirim hub'ları güvenlik modeli] belge.

### <a name="how-should-i-handle-sensitive-payload-in-push-notifications"></a>Anında iletme bildirimleri hassas yükteki nasıl işlemesi gerekir?

Tüm bildirimler, platformun PNS tarafından hedef cihazlara teslim edilir. Azure Notification hubs'ı bir bildirim gönderilir, işlenen ve ilgili PNS için geçirildi.

Azure Notification hubs'ı PNS gönderene gelen tüm bağlantıları HTTPS kullanır.

> [!NOTE]
> Azure Notification hubs'ı iletileri yükü herhangi bir şekilde günlüğe kaydetmez.

Hassas yüklerini göndermek için bir güvenli gönderme deseni kullanmanızı öneririz. Gönderen hassas yükü olmadan aygıta ping bildirim iletisi tanımlayıcıya sahip sunar. Cihazdaki uygulama yükü aldığında, uygulamayı doğrudan ileti ayrıntılarını getirmek için güvenli bir API çağrısı. Bu düzeni nasıl uygulayacağınıza karar bir kılavuz için Git [Bildirim hub'ları güvenli bildirim Öğreticisi] sayfası.

## <a name="operations"></a>İşlemler

### <a name="what-support-is-provided-for-disaster-recovery"></a>Olağanüstü durum kurtarma için hangi destek sağlanır?

Meta veri olağanüstü durum kurtarma kapsamı bizim tarafımızda (Notification hubs'ı adı, bağlantı dizesi ve diğer önemli bilgilerin) sunuyoruz. Bir olağanüstü durum kurtarma senaryosuna tetiklendiğinde kayıt verilerdir *yalnızca segmentlere* kaybolur Notification hubs'ı altyapısının. Bu veriler, yeni hub'ı kurtarma sonrası yeniden doldurmak için bir çözümü uygulamak şunlar gerekir:

1. Farklı veri merkezinde bir ikincil bildirim hub'ı oluşturun. Bir yönetim özelliklerinizi etkileyebilecek bir olağanüstü durum kurtarma olayından yüklerden baştan oluşturmanızı öneririz. Olağanüstü durum kurtarma olayın birer de oluşturabilirsiniz.

2. İkincil bildirim hub'ı birincil bildirim hub'ınıza kayıtları ile doldurun. Her iki hubs kayıtları korumak ve kayıtları sunulur eşitlenmiş durumda kalmasını çalışılırken önerilmemektedir. Bu yöntem, PNS tarafında süresi dolacak şekilde devralınan eğilimi de kayıtların nedeniyle çalışmıyor. Bildirim hub'ları temizler bunları süresi dolmuş veya geçersiz kayıtlar hakkında PNS geri bildirim aldıkça.  

Uygulama arka uçları için iki önerileri sunuyoruz:

* Kayıtları, sonunda verilen bir dizi tutan bir uygulama arka ucu kullanın. İkincil bir bildirim hub'ına toplu ekleme daha sonra gerçekleştirebilirsiniz.
* Yedek olarak birincil bildirim hub'ından kayıtları normal bir dökümünü alır bir uygulama arka ucu kullanın. İkincil bir bildirim hub'ına toplu ekleme daha sonra gerçekleştirebilirsiniz.

> [!NOTE]
> Standart katmanda kullanılabilen kayıtları içeri/dışarı aktarma işlevler açıklanan [Kayıtları dışarı/içeri aktarma] belge.

Hedef cihazlarda uygulama başlatıldığında bir arka uç yoksa, ikincil bir bildirim hub'ında yeni bir kaydı gerçekleştirin. Sonuçta ikincil bildirim hub'ı kayıtlı etkin cihaz olacaktır.

Ne zaman açılmamış bir uygulamalar içeren cihazları bildirimleri almazsınız bir zaman dilimi olur.

### <a name="is-there-audit-log-capability"></a>Denetim günlüğü özelliği var mı?

Evet. Tüm Notification hubs'ı yönetim işlemleri güncelleştirme, Azure etkinlik günlüğü sunulmuştur [Azure portal]. Azure etkinlik günlüğü aboneliklerinizdeki kaynakları üzerinde gerçekleştirilen işlemleri hakkında Öngörüler sunar. Etkinlik günlüğü'nü kullanarak ne, belirleyebilirsiniz kim ve ne zaman tüm yazma işlemlerini (PUT, POST, DELETE), aboneliğinizdeki kaynaklar için yapılan. Ayrıca, işlemleri ve diğer ilgili özellikler durumunu anlayabilirsiniz. Ancak. Etkinlik günlüğü işlemi okuma (GET) içermez.

## <a name="monitoring-and-troubleshooting"></a>İzleme ve sorun giderme

### <a name="what-troubleshooting-capabilities-are-available"></a>Sorun giderme hangi özellikler sağlanıyor?

Azure Notification hubs'ı, bırakılan bildirimler özellikle en yaygın senaryo için sorun giderme için birçok özellik sunar. Ayrıntılar için bkz [Bildirim hub'ları sorunlarını giderme] teknik incelemesi.

### <a name="what-telemetry-features-are-available"></a>Hangi telemetri özellikleri var mı?

Azure Notification hubs'ı etkinleştirir telemetri verileri görüntüleme [Azure portal]. Ölçüm ayrıntıları bulunur [bildirim hub'ları ölçümleri] sayfası.

Ölçümleri program aracılığıyla da erişebilirsiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [.NET ile Azure İzleyici ölçümleri alma](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/). Bu örnek, kullanıcı adı ve parola kullanır. Bir sertifika kullanmak için gösterildiği gibi bir sertifika sağlamanız FromServicePrincipal yöntemi aşırı yüklemek [Bu örnek](https://github.com/Azure/azure-libraries-for-net/blob/master/src/ResourceManagement/ResourceManager/Authentication/AzureCredentialsFactory.cs). 
- [Ölçümlere ve etkinlik günlükleri için bir kaynak alma](https://azure.microsoft.com/resources/samples/monitor-dotnet-query-metrics-activitylogs/)
- [Azure REST API izleme Kılavuzu](../azure-monitor/platform/rest-api-walkthrough.md)


> [!NOTE]
> Başarılı bildirimler yalnızca dış PNS (örneğin, APNS için Apple) veya Google için FCM için anında iletme bildirimleri teslim edilmediği anlamına gelir. Bildirimleri hedef cihazlara teslim PNS sorumluluğundadır. Genellikle PNS teslim ölçümleri üçüncü taraflara kullanıma sunmuyor.  

[Azure portal]: https://portal.azure.com
[Notification Hubs fiyatlandırması]: https://azure.microsoft.com/pricing/details/notification-hubs/
[Notification Hubs SLA]: https://azure.microsoft.com/support/legal/sla/
[Notification hubs'ı REST API'leri]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[Notification hubs'ı kullanmaya başlama öğreticileri]: https://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Chrome uygulama Öğreticisi]: https://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: https://azure.microsoft.com/pricing/details/mobile-services/
[Arka uç kayıt Kılavuzu]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Arka uç kaydı Kılavuzu 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[Bildirim hub'ları güvenlik modeli]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[Bildirim hub'ları güvenli bildirim Öğreticisi]: https://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[Bildirim hub'ları sorunlarını giderme]: https://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[Bildirim hub'ları ölçümleri]: ../azure-monitor/platform/metrics-supported.md#microsoftnotificationhubsnamespacesnotificationhubs
[Kayıtları dışarı/içeri aktarma]: https://docs.microsoft.com/azure/notification-hubs/export-modify-registrations-bulk
[Azure portal]: https://portal.azure.com
[complete samples]: https://github.com/Azure/azure-notificationhubs-samples
[Mobile Apps]: https://azure.microsoft.com/services/app-service/mobile/
[App Service fiyatlandırması]: https://azure.microsoft.com/pricing/details/app-service/
