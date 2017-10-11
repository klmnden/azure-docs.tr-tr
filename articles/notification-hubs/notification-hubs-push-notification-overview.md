---
title: Azure Notification Hubs
description: "Azure Notification Hubs ile anında iletme bildirimi özellikleri eklemeyi öğrenin."
author: ysxu
manager: erikre
editor: 
services: notification-hubs
documentationcenter: 
ms.assetid: fcfb0ce8-0e19-4fa8-b777-6b9f9cdda178
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 1/17/2017
ms.author: yuaxu
ms.openlocfilehash: a1be0b13cd1feb582a23965df142e44d90ac6851
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs"></a>Azure Notification Hubs
## <a name="overview"></a>Genel Bakış
Azure bildirim hub'ları, kullanımı kolay, çok platformlu, ölçeği itme altyapısı sağlar. Bir tek platformlar arası API çağrısı ile tüm Bulut veya şirket içi arka ucundan herhangi bir mobil platforma kolayca hedeflenen ve kişiselleştirilmiş anında iletme bildirimleri gönderebilirsiniz.

Bildirim hub'ları hem kuruluş hem de tüketici senaryoları için harika çalışır. Müşteriler için Notification Hubs kullanma birkaç örnek aşağıda verilmiştir:

* Düşük gecikme ile milyonlara son dakika haberi bildirimleri gönderin.
* İlginizi kullanıcı segmentlerine konum temelli kuponlar gönderin.
* Kullanıcılara veya gruplara medya/Spor/Finans/oyun uygulamaları için olay ile ilgili bildirimler Gönder.
* Promosyon içeriğini göster ve müşterilere pazar için uygulamalar iletin.
* Yeni iletiler gibi Kurumsal olaylar kullanıcıları bilgilendirin ve iş öğeleri.
* Çok faktörlü kimlik doğrulama kodları gönderin.

## <a name="what-are-push-notifications"></a>Anında İletme Bildirimleri nedir?
Anında iletme bildirimleri olan bir form uygulama kullanıcı iletişim nerede mobil uygulamaları kullanıcılarının belirli istenen bilgileri, genellikle bir açılır pencere veya iletişim kutusu size bildirilir. Kullanıcılar görüntülemek veya iletiyi kapatmak genellikle seçebilir ve eski seçme bildirim iletilen mobil uygulama açılacaktır.

Anında iletme bildirimleri güncel iş bilgilerini iletişim Kurumsal uygulamaları ve uygulamaya katılımı ve kullanımı artan tüketici uygulamaları için önemli. İlgili uygulamalar etkin olmasa da enerji tasarrufu sağlayan bildirimleri Göndericiler için esnek olarak, mobil cihazlar için kullanılabilir olduğundan ve en iyi uygulama kullanıcı iletişimini değil.

Birkaç popüler platformlar için anında iletme bildirimleri hakkında daha fazla bilgi için:
* [iOS](https://developer.apple.com/notifications/)
* [Android](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)
* [Windows](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx)

## <a name="how-push-notifications-work"></a>Anında İletme Bildirimleri Nasıl Çalışır?
Anında iletme bildirimleri adlı platforma özgü altyapılar aracılığıyla teslim edilir *Platform bildirim sistemleri* (PNSes). Bunlar, bir cihaza bir sağlanan teslim iletiye barebone itme işlevler işlemek ve ortak arabirim içermez sunar. İOS, Android ve Windows tüm müşteriler için bildirim göndermek için bir uygulama geliştiricisi sürümlerini APNS (Apple Push Notification hizmeti), FCM (Firebase Cloud Messaging) ve WNS (Windows bildirim hizmeti) ile gönderir toplu işleme sırasında çalışmanız gerekir.

Yüksek bir düzeyde İşte nasıl anında iletme çalışır:

1. İstemci uygulaması istediği bildirim almaya karar verir. Bu nedenle, benzersiz ve geçici gönderim tanıtıcısını almak için karşılık gelen PNS'ye bağlanır. Tanıtıcı türü (APNS belirteçleri sahipken ör WNS URI'dir) sistemine bağlıdır.
2. İstemci uygulaması bu tanıtıcıyı uygulama arka ucu veya sağlayıcı depolar.
3. Bir anında iletme bildirimi göndermek için uygulama arka uç belirli istemci uygulaması hedeflemek için tanıtıcıyı kullanarak PNS'ye bağlanır.
4. PNS, tanıtıcı tarafından belirtilen cihaza bildirimi iletir.

![][0]

## <a name="the-challenges-of-push-notifications"></a>Anında İletme Bildirimlerinin Zorlukları
PNSes güçlü olsa da, bunlar kadar iş uygulama geliştiricisine yayın veya segmentli kullanıcılarına anında iletme bildirimleri gönderme gibi bile ortak anında iletme bildirimi senaryolarını uygulamak için bırakın.

Anında iletme çalışma, uygulamanın ana iş mantığı ilgisiz karmaşık altyapılar gerektirdiğinden mobil bulut Hizmetleri'nde, en çok istenen özelliklerden biridir. İnfrastructural zorluklarından bazıları şunlardır:

* **Platform bağımlılığı**: 

  * Arka uç PNSes değil birleşik gibi çeşitli platformlarda cihazları bildirimleri göndermek için karmaşık ve sabit korumak platforma bağımlı mantığı olmalıdır.
* **Ölçek**:

  * PNS yönergelerine göre her uygulama başlatmada cihaz belirteçleri yenilenmelidir. Bu, yüksek miktarda trafiği ve veritabanı erişimi yalnızca belirteçlerini güncel tutmak için arka uç ilgilenme anlamına gelir. Cihaz sayısı yüzlerce ve milyonlarca binlerce büyürken oluşturma ve bu altyapısını sürdürme yoğun maliyetidir.
  * Çoğu PNSes birden çok aygıt yayın desteklemez. Bu, bir milyon aygıtları PNSes bir milyon çağrıları sonuçlarında basit bir yayın anlamına gelir. Bu trafik miktarı en düşük gecikme süresi ile ölçeklendirme ölçeklendirebilmek kolay değildir.
* **Yönlendirme**:
  
  * PNSes cihazlara iletileri göndermek için bir yol sağlar ancak çoğu uygulamaları bildirimleri kullanıcılara veya ilgi alanı gruplarına hedeflenir. Bu arka uç ilgi alanı gruplarına, kullanıcılar, özellikler, vb. ile cihazları ilişkilendirmek için bir kayıt defteri korumalıdır anlamına gelir. Bu ek yükü, bir uygulamanın Pazar ve bakım maliyetlerinin süresi ekler.

## <a name="why-use-notification-hubs"></a>Neden Notification Hubs Kullanmalıyım?
Bildirim hub'ları ortadan kaldırır, etkinleştirme ile ilişkili tüm karmaşıklık push kendi. Kendi çok platformlu, ölçeği anında iletme bildirimi altyapısı itme ilgili kodları azaltır ve arka basitleştirir. Bildirim hub'larıyla aygıtları arka uç iletileri kullanıcılara veya ilgi alanı gruplarına, aşağıdaki çizimde gösterildiği gibi gönderirken kendi PNS tanıtıcılarını bir hub ile kaydetmekten yalnızca sorumlu şunlardır:

![][1]

Bildirim hub'ları aşağıdaki avantajlara sahip kullanıma hazır itme altyapınız şöyledir:

* **Platformlar arası**

  * İOS, Android, Windows ve Kindle ve Baidu dahil tüm büyük gönderme platformlar için destek.
  * Hiçbir platforma özgü iş platforma özgü veya platformdan bağımsız biçimlerde tüm platformlar için göndermek için ortak bir arabirim.
  * Aygıt Yönetimi tek bir yerde işleyin.
* **Arka uçlarını arası**
  
  * Bulut veya şirket içi
  * .NET, Node.js, Java, vs.
* **Zengin teslim düzeni kümesi**:

  * *Bir veya birden çok platform için yayın*: platformlarda tek bir API çağrısı ile milyonlarca cihazı için anında yayınlayabilirsiniz.
  * *İtme aygıta*: bildirimleri tek tek cihazlara hedefleyebilirsiniz.
  * *İtme kullanıcıya*: etiketleri ve şablonları özellikleri, bir kullanıcının tüm platformlar arası cihazlara ulaşmak yardımcı olur.
  * *Anında iletme kesimine dinamik etiketlerle*: etiketler özelliği yardımcı, segment aygıtları ve onlara anında iletme gereksinimlerinize bağlı olarak, bir kesim veya kesimleri (örneğin etkin ve yaşamlarını değil Seattle yeni kullanıcı) bir ifade gönderiyor olup olmadığını. Pub-sub sınırlı kalmayarak yerine, herhangi bir yere aygıt etiketleri güncelleştirebilir ve zaman.
  * *Yerelleştirilmiş itme*: şablonları özelliği, arka uç kodunu etkilemeden yerelleştirme elde yardımcı olur.
  * *Sessiz anında*: sağlar gönderme çekme düzeni cihazlara sessiz bildirim göndermek ve bunları belirli çeken veya Eylemler tamamlamak için tetikleme.
  * *Zamanlanmış itme*: dilediğiniz zaman bildirimleri göndermek için zamanlayabilirsiniz.
  * *Doğrudan itme*: hizmetimizi kayıt aygıtlarla atlayın ve doğrudan toplu olarak cihaz tanıtıcılarını listesine gönderme.
  * *Kişiselleştirilmiş anında iletme*: cihaz bildirme değişkenleri aygıta özgü gönderdiğiniz yardımcı kişiselleştirilmiş anında iletme bildirimleri özelleştirilmiş anahtar-değer çiftleri ile.
* **Zengin telemetri**
  
  * Azure portalı ve program aracılığıyla genel anında iletme, aygıt, hata ve işlem telemetri yok.
  * İleti Telemetri her gönderme hizmetimizi başarıyla çıkışı iter toplu işleme için ilk isteği çağrısından izler.
  * Platform bildirim sistemi geri bildirim Platform bildirim sistemleri hata ayıklamaya yardımcı olmak üzere tüm görüşleri iletişim kurar.
* **Ölçeklenebilirlik** 
  
  * Hızlı ileti mimarisinin yeniden düzenlenmesinden veya aygıt parçalama kalmadan milyonlarca cihaza gönderebilir.
* **Güvenlik**

  * Paylaşılan erişim gizli anahtarı (SAS) veya Federasyon kimlik doğrulaması.

## <a name="integration-with-app-service-mobile-apps"></a>App Service Mobile Apps ile Tümleştirme
Azure hizmetleri genelinde kesintisiz ve birleştirici bir deneyimi kolaylaştırmak amacıyla [App Service Mobile Apps]'in Notification Hubs'ı kullanan anında iletme bildirimleri için yerleşik desteği mevcuttur. [App Service Mobile Apps], Kurumsal Geliştiriciler ve Sistem Tümleştiricileri için mobil geliştiricilere zengin bir özellik kümesi sağlayan, yüksek düzeyde ölçeklenebilir, global olarak kullanılabilir bir mobil uygulama geliştirme platformu sunar.

Mobile Apps geliştiricileri Notification Hubs'ı aşağıdaki iş akışı ile kullanabilir:

1. Cihaz PNS tanıtıcısını alma
2. Uygun Mobile Apps istemci SDK'sı kayıt API'si yoluyla Notification hubs'a cihazı kaydedin
   * Mobile Apps'in güvenlik amacıyla kayıtlardaki tüm etiketleri kaldırdığını unutmayın. Etiketleri cihazlarla ilişkilendirmek için doğrudan arka ucunuzdan Notification Hubs ile çalışın.
3. Uygulama arka ucunuzdan Notification Hubs ile bildirimler gönderme

Bu tümleştirme ile geliştiricilere sağlanan bazı kolaylıklar şunlardır:

* **Mobile Apps istemci SDK'ları**: Bu çoklu platform SDK'ları kayıt için basit API'ler sağlar ve otomatik olarak mobil uygulama ile bağlantılı bildirim hub'ına konuşun. Geliştiricilerin Notification Hubs kimlik bilgilerini sorgulaması ve ek bir hizmet ile çalışması gerekmez.

  * *İtme kullanıcıya*: SDK'ları otomatik olarak Mobile Apps kimliği doğrulanmış kullanıcı senaryosuna gönderimi sağlamak için kullanıcı kimliği ile verilen cihaz etiketi.
  * *İtme aygıta*: SDK'ları otomatik olarak Mobile Apps yükleme kimliği GUID olarak geliştiricileri birden çok hizmet GUID'i saklama zahmetinden kurtarır Notification Hubs ile kaydetmek için kullanın.
* **Yükleme modeli**: anında iletme bildirimi Hizmetleri ile hizalanan ve kullanımı kolay olan bir JSON yüklemesi bir aygıt ile ilişkili tüm gönderim özelliklerini temsil etmek için Notification Hubs en son gönderim modeli ile birlikte Mobile Apps çalışır.
* **Esneklik**: geliştiriciler yerinde tümleştirme söz konusu bile Notification Hubs ile doğrudan çalışmak her zaman seçebilirsiniz.
* **Tümleşik deneyim [Azure portal]**: bir özelliği mobil uygulamalarda görsel olarak temsil edilir ve geliştiriciler Mobile Apps aracılığıyla ilişkili bildirim hub'ı ile kolayca çalışabilir gibi anında.

## <a name="next-steps"></a>Sonraki Adımlar
Bu konu başlıklarında Notification Hubs hakkında daha fazla bilgi bulabilirsiniz:

* **[Müşteriler Notification Hubs'ı nasıl kullanıyor?]**
* **[Notification Hubs eğiticileri ve kılavuzları]**
* **Bildirim hub'ları başlama öğreticileri**: [iOS], [Android], [Windows Evrensel], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android]

[0]: ./media/notification-hubs-overview/registration-diagram.png
[1]: ./media/notification-hubs-overview/notification-hub-diagram.png
[Müşteriler Notification Hubs'ı nasıl kullanıyor?]: http://azure.microsoft.com/services/notification-hubs
[Notification Hubs eğiticileri ve kılavuzları]: http://azure.microsoft.com/documentation/services/notification-hubs
[iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
[Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
[Windows Evrensel]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
[Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
[Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
[Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
[Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
[Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
[Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
[App Service Mobile Apps]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
[templates]: notification-hubs-templates-cross-platform-push-messages.md
[Azure portal]: https://portal.azure.com
[tags]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
