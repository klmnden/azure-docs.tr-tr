<properties
    pageTitle="Azure Notification Hubs"
    description="Azure'da anında iletme bildirimlerinin nasıl kullanılacağını öğrenin. .NET API kullanarak C# dilinde yazılan kod örnekleri."
    authors="wesmc7777"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="hero-article"
    ms.date="08/25/2016"
    ms.author="wesmc"/>


#Azure Notification Hubs

##Genel Bakış

Azure Notification Hubs herhangi bir arka uçtan (bulutta veya şirket içi) herhangi bir mobil platforma mobil anında iletme bildirimleri göndermenize olanak tanıyan kullanımı kolay, çok platformlu, ölçeği genişletilmiş bir gönderim altyapısı sağlar.

Notification Hubs ile kolayca, farklı platform bildirim sistemlerinin (PNS) ayrıntılarını özetleyen, platformlar arası, kişiselleştirilmiş anında iletme bildirimleri gönderebilirsiniz. Tek bir API çağrısı ile bireysel kullanıcıları veya tüm cihazlarıyla birlikte milyonlarca kullanıcıyı içeren hedef kitle segmentlerini tümüyle hedefleyebilirsiniz.

Notification Hubs'ı hem kuruluş, hem de tüketici senaryoları için kullanabilirsiniz. Örneğin:

- Düşük gecikme ile milyonlara son dakika haberi bildirimleri gönderin (Notification Hubs, tüm Windows ve Windows Phone cihazlara önceden yüklenmiş Bing uygulamalarını çalıştırır).
- Kullanıcı segmentlerine konum temelli kuponlar gönderin.
- Spor/finans/oyun uygulamaları için kullanıcılara veya gruplara olay bildirimleri gönderin.
- Yeni iletiler/e-postalar ve satış fırsatları gibi kurumsal olaylar hakkında kullanıcılara bildirimde bulunun.
- Çok faktörlü kimlik doğrulaması için gereken bir kerelik parolalar gönderin.



##Anında İletme Bildirimleri nedir?

Akıllı telefonlar ve tabletler bir olay olduğunda kullanıcılara "bildirimde bulunabilir". Bu bildirimler birçok biçimde olabilir.

Windows Mağazası'nda ve Windows Phone uygulamalarında bu bir _bildirim_ biçiminde olabilir: Yeni bir bildirimi haber vermek için sesle birlikte engelleyici olmayan bir pencere görüntülenir. Desteklenen diğer bildirim türleri _kutucuk_ bildirimi, _ham_ bildirimler ve _gösterge_ bildirimleridir. Windows cihazlarında desteklenen bildirim türleri hakkında daha fazla bilgi için bkz. [Kutucuklar, Göstergeler ve Bildirimler](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx).

Apple iOS cihazlarda gönderim, kullanıcıdan bildirimi görüntülemesini veya kapatmasını isteyen bir iletişim kutusu ile kullanıcıya benzer şekilde bildirimde bulunur. **Görüntüle**'ye tıklandığında iletiyi alan uygulama açılır. iOS Bildirimleri hakkında daha fazla bilgi için bkz. [iOS Bildirimleri](http://go.microsoft.com/fwlink/?LinkId=615245).

Anında iletme bildirimleri, mobil cihazların enerji tasarruflarını koruyarak yeni bilgileri görüntülemesine yardımcı olur. Bildirimler, bir cihazdaki ilgili uygulamalar etkin olmasa bile arka uç sistemleri tarafından mobil cihazlara gönderilebilir. Anında iletme bildirimleri, uygulamaya katılımı ve kullanımı artırmak amacıyla kullanıldıkları tüketici uygulamaları için önemli bir bileşendir. Bildirimler aynı zamanda, güncel bilgilerin çalışanların iş olaylarına yanıt verme hızını artırdığı durumlarda kuruluşlar için de kullanışlı olabilir.

Mobil katılım senaryoları ile ilgili belirli bazı örnekler şunlardır:

1.  Windows 8 veya Windows Phone'daki bir kutucuğu geçerli finansal bilgilerle güncelleştirme.
2.  İş akışı tabanlı bir kuruluş uygulamasında, kullanıcıyı kendisine bir iş öğesi atandığını belirten bir bildirimle uyarma.
3.  Bir CRM uygulamasında (Microsoft Dynamics CRM gibi), geçerli satış fırsatları sayısını içeren bir gösterge görüntüleme.

##Anında İletme Bildirimleri Nasıl Çalışır?

Anında iletme bildirimleri, _Platform Bildirim Sistemleri_ (PNS) adlı platforma özgü altyapılar aracılığıyla teslim edilir. PNS temel işlevleri sunar (yani yayın ve kişiselleştirme desteği yoktur) ve ortak arabirim içermez. Örneğin, bir Windows Mağazası uygulamasına bildirim göndermek için geliştiricinin WNS (Windows Bildirim Hizmeti) ile iletişim kurması gerekir. Bir iOS cihazına bildirim göndermek için aynı geliştiricinin APNS (Apple Anında İletilen Bildirim Servisi) ile iletişim kurması ve iletiyi ikinci kez göndermesi gerekir. Azure Notification Hubs, her bir platformda anında iletme bildirimlerini desteklemek için diğer özelliklerle birlikte ortak bir arabirim sağlayarak yardımcı olur.

Bununla birlikte, yüksek düzeyde tüm platform bildirim sistemleri aynı düzeni uygular:

1.  İstemci uygulaması _tanıtıcısını_ almak için PNS'ye bağlanır. Tanıtıcı türü sisteme göre değişir. WNS için bu bir URI veya "bildirim kanalı"dır. APNS için bu bir belirteçtir.
2.  İstemci uygulaması bu tanıtıcıyı, daha sonra kullanılmak üzere _arka uç_'ta depolar. WNS için arka uç genellikle bir bulut hizmetidir. Apple için sistem _sağlayıcı_ olarak adlandırılır.
3.  Anında iletilen bildirim göndermek için, uygulama arka ucu belirli bir istemci uygulaması örneğini hedeflemek amacıyla tanıtıcıyı kullanarak PNS'ye bağlanır.
4.  PNS, tanıtıcı tarafından belirtilen cihaza bildirimi iletir.

![][0]

##Anında İletme Bildirimlerinin Zorlukları

Bu sistemler son derece güçlüdür ancak segmentlere ayrılmış kullanıcılara anında iletme bildirimleri yayımlamak veya göndermek gibi genel anında iletme bildirimi senaryolarını uygulamak için bile uygulama geliştiricisine çok iş bırakır.

Anında iletme bildirimleri, bulut hizmetlerinde mobil uygulamalar için en çok istenen özelliklerden biridir. Bunun nedeni, bunları çalıştırmak için gerekli altyapının oldukça karmaşık ve genellikle uygulamanın ana iş mantığı ile ilgisiz olmasıdır. İsteğe bağlı bir gönderim altyapısı oluşturmanın zorluklarından bazıları şunlardır:

- **Platform bağımlılığı.** Farklı platformlar üzerindeki cihazlara bildirim göndermek için, arka uçta birden çok arabirim kodlanmış olmalıdır. Alt düzey ayrıntıların farklı olmasının yanı sıra bildirimin sunumu da (kutucuk, bildirim veya gösterge) platforma bağımlıdır. Bu farklılıklar, karmaşık ve korunması zor arka uç kodlarına yol açabilir.

- **Ölçeklendirme.** Bu altyapıyı ölçeklendirmenin iki boyutu vardır:
    + PNS yönergelerine göre, uygulama her başlatıldığında cihaz belirteçleri yenilenmelidir. Bu, yalnızca cihaz belirteçlerini güncel tutmak için büyük miktarda trafiğe (ve izleyen veritabanı erişimlerine) yol açar. Cihazların sayısı arttığında (milyonlara ulaşabildiğinde), bu altyapıyı oluşturma ve koruma maliyeti göz ardı edilemez.

    + Çoğu PNS, birden fazla cihaza yayın yapmayı desteklemez. Dolayısıyla milyonlarca cihaza yapılan bir yayın PSN'ler için milyonlarca çağrı anlamına gelir. Uygulama geliştiriciler genellikle toplam gecikme süresini düşük tutmak istediğinden bu isteklerin ölçeklenebilmesi kolay değildir. Örneğin, iletiyi alacak olan son cihazın bildirimlerin gönderilmesinden sonraki 30 dakika boyunca bildirimi almaması gerekir, aksi bir durum anında iletme bildirimlerinin amacını boşa çıkarır.
- **Yönlendirme.** PNS'ler bir cihaza ileti göndermek için yol sunar. Bununla birlikte, çoğu uygulamada bildirimlerle kullanıcılar ve/veya ilgi alanı grupları hedeflenir (Örneğin, belirli bir müşteri hesabına atanan tüm çalışanlar). Bu nedenle bildirimleri doğru cihazlara yönlendirmek için, ilgi alanı gruplarını cihaz belirteçleri ile ilişkilendiren bir kayıt defterinin uygulama arka ucunda saklanması gerekir. Bu ek yük, bir uygulamanın toplam pazara sunum süresine ve bakım maliyetlerine eklenir.

##Neden Notification Hubs Kullanmalıyım?

Notification Hubs karmaşıklığı ortadan kaldırır: Anında iletme bildirimlerinin zorluklarını yönetmek zorunda kalmazsınız. Bunun yerine bir Bildirim Hub'ı kullanabilirsiniz. Notification Hubs tam bir çok platformlu, ölçeği genişletilmiş bir anında iletme bildirimi altyapısı kullanır ve uygulama arka ucunda çalışan gönderime özgü kodu önemli ölçüde kısaltır. Notification Hubs, bir gönderim altyapısının tüm işlevlerini uygular. Aşağıdaki şekilde gösterildiği gibi, cihazlar yalnızca PNS tanıtıcılarını kaydetmekten, arka uç ise kullanıcılara veya ilgi alanı gruplarına platformdan bağımsız iletiler göndermekten sorumludur:

![][1]


Bildirim hub'ları, aşağıdaki avantajlara sahip kullanıma hazır bir anında iletme bildirimi altyapısı sağlar:

- **Birden çok platform.**
    +  Tüm önemli mobil platformlar için destek. Bildirim hub'ları; Windows Mağazası'na, iOS, Android ve Windows Phone uygulamalarına anında iletme bildirimleri gönderebilir.

    +  Bildirim hub'ları, desteklenen tüm platformlara bildirim göndermek için ortak bir arabirim sağlar. Platforma özgü protokoller gerekli değildir. Uygulama arka ucu, platforma özgü veya platformdan bağımsız bildirimler gönderebilir. Uygulama yalnızca Notification Hubs ile iletişim kurar.

    +  Cihaz tanıtıcısı yönetimi. Notification Hubs, PNS'lerin tanıtıcı kayıt defterini ve geri bildirimlerini saklar.

- **Tüm arka uçlar ile çalışır**: Bulut veya şirket içi, .NET, PHP, Java, Node vs.

- **Ölçeklendirme.** Bildirim hub'ları, mimarinin yeniden düzenlenmesine veya parçalara ayırmaya gerek kalmadan milyonlarca cihaza ölçeklendirilir.


- **Zengin teslim düzeni kümesi**:

    - *Yayın*: Tek bir API çağrısı ile milyonlarca cihaza neredeyse eş zamanlı yayına izni verir.

    - *Tek Noktaya Yayın/Çok Noktaya Yayın*: Tüm cihazları dahil olmak üzere bütün bireysel kullanıcıları veya daha geniş bir grubu temsil eden etiketlere gönderim. Örneğin, ayrı form faktörleri (tablet - telefon).

    - *Segmentlere Ayırma*: Etiket ifadeleri tarafından tanımlanan karmaşık segmente gönderim (New York'ta Yankees'i izleyen cihazlar gibi).

    Her bir cihaz, bir bildirim hub'ına tanıtıcısını gönderirken, bir veya daha fazla _etiketi_ belirtebilir. Daha fazla bilgi için bkz. [etiketler]. Etiketlerin ön sağlamasının yapılması veya elden çıkarılması gerekmez. Etiketler, kullanıcılara veya ilgi alanı gruplarına bildirimler göndermek için basit bir yol sağlar. Etiketler uygulamaya özgü bir tanımlayıcı içerebileceğinden (kullanıcı veya grup kimliği gibi) kullanımları, uygulama arka ucunu cihaz tanıtıcılarını depolama ve yönetme yükünden kurtarır.

- **Kişiselleştirme**: Arka uç kodunu etkilemeden cihaz başına yerelleştirmeyi ve kişiselleştirmeyi gerçekleştirmek için, her bir cihazın bir veya birden çok şablonu olabilir.

- **Güvenlik**: Paylaşılan Erişim Gizli Anahtarı (SAS) veya şirket dışı kimlik doğrulaması.

- **Zengin telemetri**: Portalda ve programlama yoluyla kullanılabilir.


##App Service Mobile Apps ile Tümleştirme

Azure hizmetleri genelinde kesintisiz ve birleştirici bir deneyimi kolaylaştırmak amacıyla [App Service Mobile Apps]'in Notification Hubs'ı kullanan anında iletme bildirimleri için yerleşik desteği mevcuttur. [App Service Mobile Apps], Kurumsal Geliştiriciler ve Sistem Tümleştiricileri için mobil geliştiricilere zengin bir özellik kümesi sağlayan, yüksek düzeyde ölçeklenebilir, global olarak kullanılabilir bir mobil uygulama geliştirme platformu sunar.

Mobile Apps geliştiricileri Notification Hubs'ı aşağıdaki iş akışı ile kullanabilir:

1. Cihaz PNS tanıtıcısını alma
2. Uygun Mobile Apps İstemci SDK'sı kayıt API'si yoluyla Notification Hubs'a cihazı ve [şablonları] kaydetme
    + Mobile Apps'in güvenlik amacıyla kayıtlardaki tüm etiketleri kaldırdığını unutmayın. Etiketleri cihazlarla ilişkilendirmek için doğrudan arka ucunuzdan Notification Hubs ile çalışın.
3. Uygulama arka ucunuzdan Notification Hubs ile bildirimler gönderme

Bu tümleştirme ile geliştiricilere sağlanan bazı kolaylıklar şunlardır:

- **Mobile Apps İstemci SDK'ları.** Bu çoklu platform SDK'ları kayıt için basit API'ler sağlar ve mobil uygulama ile bağlantılı bildirim hub'ı ile otomatik olarak konuşur. Geliştiricilerin Notification Hubs kimlik bilgilerini sorgulaması ve ek bir hizmet ile çalışması gerekmez.
    + SDK'lar, kullanıcı senaryosuna gönderimi sağlamak için, belirli bir cihazı otomatik olarak Mobile Apps kimliği doğrulanmış Kullanıcı Kimliği ile etiketler.
    + SDK'lar, Notification Hubs'a kaydetmek için Mobile Apps Yükleme Kimliği'ni otomatik olarak GUID gibi kullanır. Böylece, geliştiricileri birden çok hizmet GUID'i saklama zahmetinden kurtarır.
    
- **Yükleme modeli.** Mobile Apps, Anında İletme Bildirimi Hizmetleri ile hizalanan ve kullanımı kolay olan bir JSON Yüklemesi'nde, bir cihaz ile ilişkili tüm gönderim özelliklerini temsil etmek için Notification Hubs'ın en son gönderim modeli ile çalışır.

- **Esneklik.** Geliştiriciler, yerinde tümleştirme söz konusu olduğunda bile her zaman için Notification Hubs ile doğrudan çalışmayı tercih edebilir.

- **[Azure Portal]'nda tümleşik deneyim.** Mobile Apps'te gönderim özelliği görsel olarak temsil edilir ve geliştiriciler Mobile Apps aracılığıyla ilişkili bildirim hub'ı ile kolayca çalışabilir.



##Sonraki Adımlar

Bu konu başlıklarında Notification Hubs hakkında daha fazla bilgi bulabilirsiniz:

+ **[Müşteriler Notification Hubs'ı nasıl kullanıyor?]**

+ **[Notification Hubs öğreticileri ve kılavuzları]**

+ **Notification Hubs ile Çalışmaya Başlama öğreticileri** ([iOS], [Android], [Windows Evrensel], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android])

Bildirim hub'ları için ilgili .NET yönetilen API başvuruları burada bulunur:

+ [Microsoft.WindowsAzure.Messaging.NotificationHub]
+ [Microsoft.ServiceBus.Notifications]


  [0]: ./media/notification-hubs-overview/registration-diagram.png
  [1]: ./media/notification-hubs-overview/notification-hub-diagram.png
  [Müşteriler Notification Hubs'ı nasıl kullanıyor?]: http://azure.microsoft.com/services/notification-hubs
  [Notification Hubs öğreticileri ve kılavuzları]: http://azure.microsoft.com/documentation/services/notification-hubs
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
  [şablonları]: notification-hubs-templates-cross-platform-push-messages.md
  [Azure Portal]: https://portal.azure.com
  [etiketler]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)



<!--HONumber=sep16_HO1-->


