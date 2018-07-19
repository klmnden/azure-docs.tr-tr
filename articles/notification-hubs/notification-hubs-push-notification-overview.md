---
title: Azure Notification Hubs nedir?
description: Azure Notification Hubs ile anında iletme bildirimi özellikleri eklemeyi öğrenin.
author: dimazaid
manager: kpiteira
editor: spelluru
services: notification-hubs
documentationcenter: ''
ms.assetid: fcfb0ce8-0e19-4fa8-b777-6b9f9cdda178
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: overview
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 44086bc20966d9c01ff27dda68f837101c71a778
ms.sourcegitcommit: 15bfce02b334b67aedd634fa864efb4849fc5ee2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "33777938"
---
# <a name="what-is-azure-notification-hubs"></a>Azure Notification Hubs nedir?
Azure Notification Hubs, herhangi bir arka uçtan (bulut ya da şirket içi) herhangi bir platforma (iOS, Android, Windows, Kindle, Baidu vb.) bildirim göndermenize olanak tanıyan, kullanımı kolay ve ölçeği artırılmış bir gönderme altyapısı sağlar. Notification Hubs hem kuruluş hem de tüketici senaryoları için sorunsuzca çalışır. Birkaç senaryo örneği aşağıda verilmiştir:

- Düşük gecikme ile milyonlarca kişiye son dakika haber bildirimleri gönderme.
- İlgili kullanıcı segmentlerine konum temelli kuponlar gönderme.
- Medya/spor/finans/oyun uygulamaları için kullanıcılara veya gruplara etkinliklerle ilgili bildirimler gönderme.
- Müşterilerle etkileşimde bulunmak ve pazarlama yapmak için uygulamalara promosyon içeriği gönderme.
- Yeni iletiler ve iş öğeleri gibi kurumsal olaylar hakkında kullanıcılara bildirim gönderme.
- Çok faktörlü kimlik doğrulaması için kod gönderme.

## <a name="what-are-push-notifications"></a>Anında iletme bildirimleri nedir?
Anında iletme bildirimleri, mobil uygulama kullanıcılarının istenen belirli bilgiler hakkında genellikle bir açılır pencere ya da iletişim kutusu içinde bildirim aldığı, uygulamadan kullanıcıya iletişim biçimidir. Kullanıcılar genellikle iletiyi görüntülemeyi veya kapatmayı seçebilir. İletiyi görüntülemeyi seçtiklerinde, bildirimi ileten mobil uygulama açılır.

Anında iletme bildirimleri, tüketici uygulamalarının uygulama katılımı ve kullanımını artırması ve kurumsal uygulamaların güncel iş bilgilerini iletmesi için çok önemlidir. Mobil cihazlar için enerji verimlili, bildirimi gönderenler için esneklik ve ilgili uygulamalar etkin olmadığında kullanılabilirlik sağladığından, uygulamadan kullanıcıya en iyi iletişim yöntemidir.

Birkaç popüler platformda anında iletme bildirimleri hakkında daha fazla bilgi için aşağıdaki konulara bakın: 
* [iOS](https://developer.apple.com/notifications/)
* [Android](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)
* [Windows](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx)

## <a name="how-push-notifications-work"></a>Anında iletme bildirimleri nasıl çalışır?
Anında iletme bildirimleri, *Platform Bildirim Sistemleri* (PNS) adlı platforma özgü altyapılar aracılığıyla teslim edilir. Bunlar, sağlanan bir tanıtıcı ile bir cihaza iletileri ulaştırmak için tenel gönderme işlevleri sunarlar ve ortak bir arabirimleri yoktur. Bir uygulamanın iOS, Android ve Windows sürümlerinde tüm müşterilere bir bildirim göndermek için geliştiricinin Apple Anında İletilen Bildirim Servisi (APNS), Firebase Cloud Messaging (FCM) ve Windows Bildirim Hizmeti (WNS) ile birlikte çalışması gerekir.

Yüksek bir düzeyde gönderme işlemi şu şekilde çalışır:

1. İstemci uygulaması, bildirimi almak isteyip istemediğine karar verir. Bu nedenle, benzersiz ve geçici gönderme tanıtıcısını almak üzere ilgili PNS ile iletişim kurar. Tanıtıcı türü, sisteme bağlıdır (örneğin, WNS’de URI’ler, APNS’de ise belirteçler bulunur).
2. İstemci uygulaması bu tanıtıcıyı uygulama arka ucunda veya sağlayıcısında depolar.
3. Anında iletilen bildirim göndermek için, uygulama arka ucu belirli bir istemci uygulamasını hedeflemek amacıyla tanıtıcıyı kullanarak PNS'ye bağlanır.
4. PNS, tanıtıcı tarafından belirtilen cihaza bildirimi iletir.

![Anında iletme bildirimi iş akışı](./media/notification-hubs-overview/registration-diagram.png)

## <a name="the-challenges-of-push-notifications"></a>Anında iletme bildirimlerinin zorlukları
PNS’ler güçlüdür. Bununla birlikte, segmentlere ayrılmış kullanıcılara anında iletme bildirimleri yayımlamak gibi genel anında iletme bildirimi senaryolarını uygulamak için bile uygulama geliştiricisine çok iş bırakır.

Anında iletme bildirimlerinin gönderilmesi için, uygulamanın ana iş mantığıyla ilgili olmayan karmaşık bir altyapı gerekir. Altyapıyla ilgili zorluklardan bazıları şunlardır:

- **Platform bağımlılığı**
    - PNS’ler birleşik olmadığından, arka ucun çeşitli platformlardaki cihazlara bildirim göndermesi için karmaşık ve bakımı zor, platforma bağımlı bir mantığa sahip olması gerekir.
- **Ölçeklendirme**
    - PNS yönergelerine göre, uygulama her başlatıldığında cihaz belirteçlerinin yenilenmesi gerekir. Arka uç, yalnızca belirteçleri güncel tutmak için büyük miktarda trafik ve veritabanı erişimi ile uğraşır. Cihazların sayısı yüzlerce ve binlerce milyona ulaştığında, bu altyapıyı oluşturma ve koruma maliyeti çok büyük olur.
    - Çoğu PNS, birden fazla cihaza yayın yapmayı desteklemez. Bir milyon cihaza basit bir yayın yapılması, PNS’lere yönelik bir milyon çağrı ile sonuçlanır. Bu miktarda trafiğin en düşük gecikme ile ölçeklendirilmesi sıradan bir işlem değildir.
- **Yönlendirme** 
    - PNS’ler cihazlara iletileri göndermek için bir yol sağlasa da, çoğu uygulama bildirimi kullanıcılara veya ilgi alanı gruplarına yöneliktir. Cihazları ilgi alanı grupları, kullanıcılar, özellikler vb. ile ilişkilendirmek için arka ucun bir kayıt defteri tutması gerekir. Bu ek yük, bir uygulamanın pazara sunum süresine ve bakım maliyetlerine eklenir.

## <a name="why-use-azure-notification-hubs"></a>Azure Notification Hubs neden kullanılır?
Notification Hubs, uygulama arka ucunuzdan kendi başınıza bildirim göndermeyle ilişkili tüm karmaşıklığı ortadan kaldırır. Çok platformlu, ölçeği genişletilmiş anında iletme bildirimi altyapısı, gönderme ile ilgili kodlama işlemlerini azaltır ve arka ucunuzu basitleştirir. Notification Hubs sayesinde cihazlar yalnızca PNS tanıtıcılarını bir hub’a kaydetmekten sorumludur. Arka uç ise aşağıdaki şekilde gösterildiği gibi kullanıcılara veya ilgili alanı gruplarına iletiler gönderir:

![Bildirim Hub'ı diyagramı](./media/notification-hubs-overview/notification-hub-diagram.png)

Bildirim hub'ları, aşağıdaki avantajlara sahip olan kullanıma hazır gönderme altyapınızdır:

- **Platformlar arası**
    - iOS, Android, Windows ve Kindle ile Baidu dahil olmak üzere başlıca tüm gönderme platformlarını destekler.
    - Platforma özel bir iş olmaksızın platforma özgü ya da platforma bağımlı biçimlerle tüm platformlara gönderebilen bir ortak arabirimdir.
    - Cihaz tanıtıcısı yönetimi tek yerden yürütülür.
- **Arka uçlar arası**
    - Bulut veya şirket içi
    - .NET, Node.js, Java vb.
- **Zengin teslim düzeni kümesi**
    - Bir veya birden çok platforma yayın: Tek bir API çağrısı ile platformlar arasında milyonlarca cihaza anında yayın yapabilirsiniz.
    - Cihaza gönderme: Bildirimlerle tek cihazları hedefleyebilirsiniz.
    - Kullanıcıya gönderme: Etiket ve şablon özellikleri, bir kullanıcının tüm platformlar arası cihazlarına ulaşmanıza yardımcı olur.
    - Dinamik etiketlerle segmente gönderme: Etiket özelliği, cihazları segmentlere ayırmanıza ve tek bir segmente ya da segment ifadesine göndermenize bakılmaksızın (örneğin, etkin VE Seattle’da yaşıyor, yeni kullanıcı DEĞİL) gereksinimlerinize göre cihazlara bildirim göndermenize yardımcı olur. Pub-sub ile kısıtlanmadan cihaz etiketlerini dilediğiniz yerde ve dilediğiniz zaman güncelleştirebilirsiniz.
    - Yerelleştirilmiş gönderme: Şablon özelliği, arka uç kodunu etkilemeden yerelleştirmeyi elde etmeye yardımcı olur.
    - Sessiz gönderme: Cihazlara sessiz bildirimler göndererek ve belirli çekme veya eylemleri tamamlamak üzere cihazları tetikleyerek gönderme-çekme düzenini etkinleştirebilirsiniz.
    - Zamanlanmış gönderme: Dilediğiniz zaman bildirimleri göndermek için zamanlayabilirsiniz.
    - Doğrudan gönderme: Notification Hubs hizmetine cihaz kaydetme işlemini atlayabilir ve doğrudan bir cihaz tanıtıcıları listesine toplu gönderim yapabilirsiniz.
    - Kişiselleştirilmiş gönderme: Cihaz gönderme değişkenleri, özelleştirilmiş anahtar-değer çiftleri ile cihaza özgü kişiselleştirilmiş anında iletme bildirimleri göndermenize yardımcı olur.
- **Zengin telemetri**
    - Azure portalında ve program aracılığıyla genel gönderme, cihaz, hata ve işlem telemetrileri kullanılabilir.
    - İleti Başına Telemetri, yaptığınız ilk istek çağrısından Notification Hubs hizmetine kadar her bir gönderme işlemini takip eder ve gönderme işlemleri başarıyla toplu hale getirir.
    - Platform Bildirim Sistemi Geri Bildirimi, hata ayıklamaya yardımcı olmak üzere Platform Bildirim Sistemi’nden alınan tüm geri bildirimleri iletir.
- **Ölçeklenebilirlik** 
    - Yeniden tasarlama veya cihaz parçalaması yapmadan milyonlarca cihaza hızlı iletiler gönderin.
- **Güvenlik**
    - Paylaşılan Erişim Gizli Dizisi (SAS) veya şirket dışı kimlik doğrulaması.

## <a name="integration-with-app-service-mobile-apps"></a>App Service Mobile Apps ile Tümleştirme
Azure hizmetleri genelinde kesintisiz ve birleştirici bir deneyimi kolaylaştırmak amacıyla [App Service Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md)'in Notification Hubs'ı kullanan anında iletme bildirimleri için yerleşik desteği mevcuttur. [App Service Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md), Kurumsal Geliştiriciler ve Sistem Tümleştiricileri için mobil geliştiricilere zengin bir özellik kümesi sağlayan, yüksek düzeyde ölçeklenebilir, global olarak kullanılabilir bir mobil uygulama geliştirme platformu sunar.

Mobile Apps geliştiricileri Notification Hubs'ı aşağıdaki iş akışı ile kullanabilir:

1. Cihaz PNS tanıtıcısını alma
2. Uygun Mobile Apps İstemci SDK'sı kayıt API'si yoluyla Notification Hubs'a cihazı kaydetme

    > [!NOTE]
    > Mobile Apps'in güvenlik amacıyla kayıtlardaki tüm etiketleri kaldırdığını unutmayın. Etiketleri cihazlarla ilişkilendirmek için doğrudan arka ucunuzdan Notification Hubs ile çalışın.
1. Uygulama arka ucunuzdan Notification Hubs ile bildirimler gönderme

Bu tümleştirme ile geliştiricilere sağlanan bazı kolaylıklar şunlardır:

- **Mobile Apps İstemci SDK’ları**: Bu çoklu platform SDK'ları kayıt için basit API'ler sağlar ve mobil uygulama ile bağlantılı bildirim hub'ı ile otomatik olarak konuşur. Geliştiricilerin Notification Hubs kimlik bilgilerini sorgulaması ve ek bir hizmet ile çalışması gerekmez.
    - *Kullanıcıya gönderme*: SDK'lar, kullanıcı senaryosuna gönderimi sağlamak için, belirli bir cihazı otomatik olarak Mobile Apps kimliği doğrulanmış Kullanıcı Kimliği ile etiketler.
    - *Cihaza gönderme*: SDK'lar, Notification Hubs'a kaydetmek için Mobile Apps Yükleme Kimliği'ni otomatik olarak GUID gibi kullanır. Böylece, geliştiricileri birden çok hizmet GUID'i saklama zahmetinden kurtarır.
- **Yükleme modeli**: Mobile Apps, Anında İletme Bildirimi Hizmetleri ile hizalanan ve kullanımı kolay olan bir JSON Yüklemesi'nde, bir cihaz ile ilişkili tüm gönderim özelliklerini temsil etmek için Notification Hubs'ın en son gönderim modeli ile çalışır.
- **Esneklik**: Geliştiriciler, yerinde tümleştirme söz konusu olduğunda bile her zaman için Notification Hubs ile doğrudan çalışmayı tercih edebilir.
- **[Azure portalında](https://portal.azure.com) tümleşik deneyim**: Mobile Apps'te gönderim özelliği görsel olarak temsil edilir ve geliştiriciler Mobile Apps aracılığıyla ilişkili bildirim hub'ı ile kolayca çalışabilir.

## <a name="next-steps"></a>Sonraki adımlar
[Öğretici: Mobil uygulamalara anında iletme bildirimleri gönderme](notification-hubs-android-push-notification-google-fcm-get-started.md) bölümünü takip ederek bildirim hub’ı oluşturmaya ve kullanmaya başlayın. [0]: ./media/notification-hubs-overview/registration-diagram.png [1]: ./media/notification-hubs-overview/notification-hub-diagram.png [Müşteriler Notification Hubs’ı nasıl kullanıyor]: http://azure.microsoft.com/services/notification-hubs [Notification Hubs öğreticileri ve kılavuzları]: http://azure.microsoft.com/documentation/services/notification-hubs [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started [Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started [Windows Evrensel]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx [App Service Mobile Apps]: https://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop/ [şablonlar]: notification-hubs-templates-cross-platform-push-messages.md [Azure portalı]: https://portal.azure.com [etiketler]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
