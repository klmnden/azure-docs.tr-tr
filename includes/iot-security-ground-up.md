---
title: include dosyası
description: include dosyası
services: iot-fundamentals
author: robinsh
ms.service: iot-fundamentals
ms.topic: include
ms.date: 04/24/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: b952763378de562f35c2e1ecaf49c56f0145c559
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64951577"
---
# <a name="security-for-internet-of-things-iot-from-the-ground-up"></a>Nesnelerin interneti (IOT) baştan için güvenlik

Nesnelerin interneti (IOT), dünya çapında işletmeler için güvenlik, gizlilik ve uyumluluk zorluk doğurur. Burada bu sorunları nasıl uygulandığını ve yazılım etrafında çalışmalarınızı geleneksel siber teknoloji, IOT siber ve fiziksel ortamdan yakınsama ne olur ilgilidir. IOT çözümleri koruma sağlayarak, bu cihazlar Bulut ve işlem ve depolama sırasında buluta güvenli veri koruma arasında güvenli bağlantı cihazların güvenli sağlama gerektirir. Bu işlevselliğin karşı çalışıyor, ancak, bir çözüm içindeki cihazlar çok sayıda kaynak kısıtlı cihazları ve dağıtımları coğrafi dağılımı uygulanır.

Bu makalede, IOT Çözüm Hızlandırıcıları güvenli ve özel bir nesnelerin interneti bulut çözümünü nasıl sağladığını açıklar. Çözüm Hızlandırıcıları baştan her aşamada yerleşik güvenlik ile eksiksiz bir uçtan uca çözümü sunun. Microsoft'ta, güvenli bir yazılım geliştirme kökü Microsoft'un yıllardır yazılım Mühendisliği uygulama, bir parçası olan uzun deneyimi güvenli yazılım geliştirme. Bunu sağlamak için bir konak, çalışma güvenliği güvencesi (OSA) ve Microsoft dijital Suçlar birimi, Microsoft gibi bir altyapı düzeyinde güvenlik hizmetleri ile birlikte, temel geliştirme metodolojisinin Security Development Lifecycle (SDL) olan Güvenlik Yanıt Merkezi ve Microsoft kötü amaçlı yazılımdan koruma Merkezi.

Çözüm Hızlandırıcıları benzersiz özellikleri IOT cihazlarından kolay ve saydam veri depolama sağlama yapma ve bağlanma sunuyor ve en önemlisi, güvenli. Bu makale Azure IOT Çözüm Hızlandırıcıları güvenlik özelliklerini inceler ve güvenlik, gizlilik ve uyumluluk sorunları emin olmak için Dağıtım stratejisi ele alınır.

## <a name="introduction"></a>Giriş

Nesnelerin interneti (IOT), gelecekteki, işletmelerin hemen sunan ve maliyetleri azaltmak için gelirinizi artırın ve kendi işletmelerini dönüştürme için gerçek fırsatları wave olur. Birçok işletme, ancak kendi kuruluşlardaki güvenlik, gizlilik ve uyumluluk ile ilgili endişelerini nedeniyle IOT dağıtmak bunu yapamıyor. Önemli önemli bir nokta oluşturulduğunda sanal ve fiziksel ortamdan birlikte, bu iki platformdan da devralınan bireysel risklerin birleşmesi, kullanılması birleştirir IOT altyapısı, gelir. IOT güvenlik ilgilidir cihaz üzerinde çalışan kod bütünlüğünü sağlamak için cihaz ve kullanıcı kimlik doğrulaması sağlamak cihazların (yanı sıra bu cihazları tarafından oluşturulan verileri) Temizle sahipliğini tanımlama ve siber ve fiziksel saldırılara karşı dayanıklı.

Ardından, gizlilik ilgili bir sorun yoktur. Saydamlık ne toplanır gibi ve neden, veri toplama, ilgili şirketler, erişim ve benzeri kontrol eden görebilen istersiniz. Son olarak, genel güvenlik sorunlarını bunları çalışan kişiler yanı sıra donanım ve Bakım endüstri standartları uyumluluk sorunları vardır.

Güvenlik, gizlilik, saydamlık ve uyumluluk sorunları verildiğinde, doğru IOT çözüm sağlayıcısı seçme, bir sınama kalır. IOT yazılım ve hizmetlerinin, çeşitli satıcılar tarafından sağlanan tek tek parçaları birlikte birleştirme, güvenlik, gizlilik, saydamlık ve düzeltmek sürelerde algılamak zor olabilecek uyumluluk, boşlukları tanıtır. Yazılım ve hizmet sağlayıcısına hangi sıralamasına koydunuz ve coğrafyalarda span, ancak aynı zamanda güvenli ve saydam bir şekilde ölçeklendirebilirsiniz hizmetleri çalıştırma kapsamlı deneyime sahip sağlayıcıları bulunması temel doğru IOT seçimi. Benzer şekilde, bu yirmi yılın üzerinde deneyime sahip makineler dünya çapında milyarlarca üzerinde çalışan güvenli yazılım geliştirme ve nesnelerin interneti tarafından bu yeni dünya teşkil tehditlere yönetilmesinde olanağına sahip ve seçilen sağlayıcıyı için yardımcı olur.

## <a name="secure-infrastructure-from-the-ground-up"></a>Baştan güvenli bir altyapı

[Microsoft Cloud](https://azure.microsoft.com) altyapısını 127 ülkelerde/bölgelerde bir milyardan fazla müşteriler destekler. Kurumsal Yazılım oluşturma ve dünyanın en büyük çevrimiçi hizmetlerinden bazılarını çalıştırma konusundaki onlarca yıllık deneyimi Microsoft'un çizme, Microsoft Cloud daha yüksek düzeyde Gelişmiş güvenlik, gizlilik, uyumluluk ve tehdit önleme uygulamalarını da sağlar Çoğu müşteri, kendi olabileceğimizden çok.

[Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/) tüm yazılım yaşam döngüsüne güvenlik gereksinimleri ekler bir zorunlu şirket çapında geliştirme süreci sağlar. Çalışma etkinliklerinin aynı güvenlik uygulamaları düzeyini izlediğinden emin olmak için Microsoft'un çalışma güvenliği güvencesi (OSA) sürecinde özenli güvenlik yönergeleri SDL kullanır. Microsoft, üçüncü taraf denetim firmaları, uyumluluk yükümlülükler karşıladığından ve Microsoft Microsoft dijital Suçlar birimi dahil olmak üzere mükemmellik merkezleri oluşturmak geniş güvenlik çalışmaları ilgilenir. devam eden bir doğrulama ile de çalışır, Microsoft Güvenlik Yanıt Merkezi ve Microsoft kötü amaçlı yazılımdan koruma Merkezi.

## <a name="microsoft-azure---secure-iot-infrastructure-for-your-business"></a>Microsoft Azure - işletmeniz için güvenli bir IOT altyapısı

Microsoft Azure, bir bulut çözümü, sürekli büyüyen bir tümleşik bulut Hizmetleri koleksiyonunu birleştiren sunar — analiz, makine öğrenimi, depolama, güvenlik, ağ ve web — ile koruma için sektör lideri bir taahhüt ve verilerinizin gizliliğine. Microsoft'un [ihlal varsayımı](https://azure.microsoft.com/blog/red-teaming-using-cutting-edge-threat-simulation-to-harden-the-microsoft-enterprise-cloud/) ayrılmış bir strateji kullanır *kırmızı takım* Azure algılama özelliğini test saldırı benzetimi kullanan yazılım güvenliği uzmanlarından oluşan Gelişmekte olan tehditlere karşı koruma ve kurtarma ihlallerinden. Microsoft'un [küresel olay yanıt](https://www.microsoft.com/en-us/TrustCenter/Security/DesignOpSecurity) sistemlerimizdeki saldırıların ve zararlı etkinliklerin etkilerini azaltmak için çalışır. Takım, olay yönetimini, iletişim ve kurtarma için yerleşik yordamları izleyen ve iç ve dış iş ortaklarıyla bulunabilir ve tahmin edilebilir arabirimleri kullanır.

Microsoft'un sistemleri sürekli izinsiz giriş algılama ve önleme, hizmet izleme saldırısını önlemeyi, düzenli sızma testi ve tehditlerin belirlenmesine ve azaltılmasına yardımcı forensic araçları sağlar. [Çok faktörlü kimlik doğrulaması](../articles/active-directory/authentication/multi-factor-authentication.md) ek bir ağa erişmek son kullanıcılar için güvenlik katmanı sağlar. Ve Microsoft uygulama ve konak sağlayıcısı için erişim denetimi, izleme, kötü amaçlı yazılımdan koruma, güvenlik açığı taraması, düzeltme ekleri ve yapılandırma yönetimi sunar.

Çözüm Hızlandırıcıları SDL ve OSA işlemleri güvenli geliştirme ve işlem, tüm Microsoft yazılımları için birlikte Azure platformda yerleşik gizlilik ve güvenlik yararlanın. Bu yordamlar, kimlik ve yönetim özellikleri herhangi bir çözüm güvenlik temel altyapıyı koruma ve ağ koruması sağlar.

[Azure IOT hub'ı](../articles/iot-hub/about-iot-hub.md) içinde [IOT Çözüm Hızlandırıcıları](../articles/iot-fundamentals/iot-introduction.md) gibi IOT cihazları ve Azure hizmetleri arasında güvenilir ve güvenli çift yönlü iletişim sağlayan ve tam olarak yönetilen bir hizmet sunar. [Azure Machine Learning](../articles/machine-learning/studio/what-is-machine-learning.md) ve [Azure Stream Analytics](../articles/stream-analytics/stream-analytics-introduction.md) cihaz başına güvenlik kimlik ve erişim denetimi kullanarak.

En iyi güvenlik ve gizlilik özellikleri yerleşik Azure IOT Çözüm Hızlandırıcıları ile iletişim kurmak için bu makalede üç birincil güvenlik buradadır suite ayırır.

![Azure IoT çözüm hızlandırıcıları](media/iot-security-ground-up/securing-iot-ground-up-fig3.png)

### <a name="secure-device-provisioning-and-authentication"></a>Cihaz sağlama ve kimlik doğrulaması güvenliğini sağlama

Bunlar alanında işleminde ederken aygıtıyla iletişim kurmak için IOT altyapısı tarafından kullanılan her cihaz için bir benzersiz kimlik anahtarı sağlayarak yokken cihazlar Çözüm Hızlandırıcıları güvenli hale getirin. Hızlı ve kolay ayarlama işlemidir. Bir kullanıcı tarafından seçilen cihaz kimliği ile oluşturulan anahtarı Azure IOT Hub ile cihaz arasındaki tüm iletişimi kullanılan belirtecin temelini oluşturur.

Cihaz kimlikleri (yani bir donanım güven modülde flashed olduğu gibi) üretim sırasında bir cihazla ilişkilendirilebilir veya mevcut bir proxy olarak (örneğin CPU seri numaraları) kimlik sabit kullanabilirsiniz. Bu cihazı tanımlama bilgileri değiştirilmesi basit olmadığından, temel alınan cihaz Donanım değişikliklerini ancak mantıksal aygıt kalan durumunda aynı mantıksal cihaz kimlikleri tanıtmak önemlidir. Bazı durumlarda, bir cihaz kimliği ilişkisini cihaz dağıtım sırasında olabilir (örneğin, bir kimliği doğrulanmış saha mühendisi fiziksel olarak yeni bir cihazın çözüm arka ucu ile iletişim kurulurken yapılandırır). [Azure IOT Hub kimlik kayıt defteri](../articles/iot-hub/iot-hub-devguide.md) cihaz kimliklerini ve çözüm için güvenlik anahtarları güvenli olarak depolanmasını sağlar. Kişi veya grup cihaz kimliklerinin bir izin verilenler listesi veya Engellenenler listesi, cihaz erişiminde tam kontrole etkinleştirme eklenebilir.

Bulutta Azure IOT hub'ı erişim denetimi ilkeleri, bir cihaz IOT dağıtımından gerektiğinde ilişkisini kaldırmak için bir yol sağlayarak, herhangi bir cihaz kimliği devre dışı bırakma ve etkinleştirme etkinleştirin. Bu ilişkilendirme ve cihazların ilişkisi kaldırma her bir cihaz kimliği dayanır.

Ek cihaz güvenlik özellikleri içerir:

* Cihazlar istenmemiş ağ bağlantılarını kabul etmeyen. Bunlar, yalnızca giden bir şekilde tüm bağlantılar ve rotalar oluşturun. Bir cihazın arka uçtan bir komut alması için cihazın tüm bekleyen komutları işlemek denetlemek için bir bağlantı başlatması gerekir. Güvenli bir şekilde cihaz ile IOT hub'ı arasında bir bağlantı kurulduktan sonra buluttan cihaza ve CİHAZDAN buluta ileti şeffaf bir şekilde gönderilebilir.

* Cihazları yalnızca bağlanmak veya ile kullanıcılar, Azure IOT Hub gibi eşlenmiş eşlendikleri hizmetlerden rotalar oluşturun.

* Sistem düzeyinde yetkilendirme ve kimlik doğrulama cihaz başına kimlik, erişim kimlik bilgileri ve izinleri neredeyse yapmadan kullanın-anında iptal edilebilir.

### <a name="secure-connectivity"></a>Güvenli bağlantı

Mesajlaşma dayanıklılık, tüm IOT çözümlerinin, önemli bir özelliğidir. Arızaya komutları sunun ve/veya cihazlardan veri almak için gereken IOT cihazları Internet veya güvenilir olmayan diğer benzer ağ üzerinden bağlı olarak çizilir. Azure IOT Hub, bulut üzerinden ve cihazlara gönderilen iletilere yanıt olarak bir sistem arasında Mesajlaşma dayanıklılık sunar. Mesajlaşma için ek dayanıklılık düzeyi, iletileri IOT hub'ında en fazla yedi gün telemetri ve komutlar için iki gün boyunca önbelleğe alarak elde edilir.

Verimliliği, kaynakları ve kaynak kısıtlı bir ortam işlem koruma sağlamak önemlidir. HTTPS (güvenli), HTTP popüler http protokolü, endüstri standardı güvenli sürümü, verimli iletişimin etkinleştirilmesi Azure IOT Hub tarafından desteklenir. Gelişmiş ileti sıraya alma Protokolü (AMQP) ve Message Queuing Telemetry Transport (Azure IOT Hub tarafından desteklenen MQTT), yalnızca kaynak kullanımının aynı zamanda güvenilir ileti teslimi açısından verimlilik için tasarlanmıştır. 

Ölçeklenebilirlik, çok çeşitli cihazları güvenli bir şekilde çalışma olanağı gerektirir. Azure IOT hub, hem IP özellikli hem de IP özellikli olmayan cihazlar için güvenli bir bağlantı sağlar. IP özellikli cihazların doğrudan bağlanmak ve güvenli bir bağlantı üzerinden IOT Hub'ınızla iletişim olanağına sahip olursunuz. IP özellikli olmayan cihazları kaynak kısıtlanmış ve yalnızca kısa bir mesafe Zwave ZigBee ve Bluetooth gibi iletişim protokolleri üzerinden bağlanın. Bir alan ağ geçidi, bu cihazları toplamak için kullanılır ve bulut ile güvenli çift yönlü iletişim sağlamak için protokol çevirisi gerçekleştirir.

Ek bağlantı güvenlik özellikleri içerir:

* Ağ geçidi ve Azure IOT Hub'ı arasında veya Azure IOT Hub ile cihazlar arasındaki iletişim yolunun, sektör standardı Aktarım Katmanı Güvenliği (TLS) kullanarak X.509 protokolünü kullanarak kimlik doğrulaması Azure IOT Hub ile sağlanır.

* Gelen istekte bulunulmamış gelen bağlantıları cihazları korumak için Azure IOT Hub cihaz herhangi bir bağlantı açılmaz. Cihaz tüm bağlantıları başlatır.

* Azure IOT Hub, arızaya iletileri cihazlar için depolar ve cihazın bağlanmak için bekler. Bu komutlar, tutularak gerçekleştirilir, bu komutları almak üzere güç veya bağlantı sorunları nedeniyle, bağlanan cihazları etkinleştirme iki gün boyunca saklanır. Azure IOT Hub, her cihaz için bir cihaz başına kuyruk oluşturur.

### <a name="secure-processing-and-storage-in-the-cloud"></a>İşlem ve depolama bulutta güvenli hale getirme

Şifreli iletişimler verileri bulutta işleme Çözüm Hızlandırıcıları verilerinin korunmasına yardımcı olur. Bu ek şifreleme ve güvenlik Anahtar Yönetimi uygulama esnekliği sağlar.

Kullanıcı kimlik doğrulaması ve yetkilendirme için Azure Active Directory (AAD) kullanarak Azure IOT Çözüm Hızlandırıcıları, denetlenir ve gözden kolay erişim yönetimini etkinleştirme verileri için bulutta, ilke tabanlı yetkilendirme modeli sağlayabilir. Bu model, ayrıca neredeyse anında iptal bulutta verilere erişim ve Azure IOT Çözüm Hızlandırıcıları için bağlı cihazların sağlar.

Bulutta veri hale geldikten sonra işlenir ve herhangi bir kullanıcı tarafından tanımlanan iş akışında depolanır. Her veri parçası için erişim, Azure Active Directory ile kullanılan depolama hizmete bağlı olarak denetlenir.

Anahtarlar kullanılması gerektiği durumlarda Atla olanağı IOT altyapısı tarafından kullanılan tüm anahtarları Güvenli Depolama, bulutta depolanır. Veri depolanabilir [Azure Cosmos DB](../articles/cosmos-db/introduction.md) veya [SQL veritabanları](../articles/sql-database/sql-database-faq.md), istenen güvenlik düzeyini tanımını etkinleştirme. Ayrıca, Azure izleme ve denetleme tüm verilerinize erişim için sizi bir yetkisiz erişim veya yetkisiz erişim için bir yol sağlar.

## <a name="conclusion"></a>Sonuç

Nesnelerin interneti nesnelerinizle başlatır; işletmeler için en önemli şeylerden. IOT harika değeri, maliyetleri azaltırken, artan gelir ve iş dönüştürme için bir iş sunabilir. Bu dönüşüm başarısını doğru IOT yazılım ve hizmet sağlayıcısı seçme üzerinde büyük ölçüde bağlıdır. Yalnızca bu dönüştürme anlama iş gereksinimlerine göre catalyzes, ancak aynı zamanda Hizmetleri ve yazılım güvenlik, gizlilik, saydamlık ve uyumluluk önemli tasarım konuları ile oluşturulmuş bir sağlayıcı bulma anlamına gelir. Microsoft geliştirme ve güvenli bir yazılım ve Hizmetleri dağıtma ile kapsamlı bir deneyimi vardır ve bu yeni nesnelerin interneti çağda lider olmaya devam eder.

Çözüm Hızlandırıcıları işletmelerin dönüştürmek için güvenli varlıkları, verimliliği artırmak için izlemeyi etkinleştirme tasarım gereği, yenilik ve gelişmiş veri analizi görevlendirmek sürücü operasyonel performans güvenlik önlemleri oluşturun. Çözüm Hızlandırıcıları, katmanlı bir yaklaşım güvenlik, birden çok güvenlik özellikleri ve tasarım desenleri, herhangi bir işletme dönüştürmek için güvenilen bir altyapı dağıtmanızı yardımcı olur.

## <a name="additional-information"></a>Ek bilgiler

Her Çözüm Hızlandırıcısı gibi Azure hizmetlerini örneklerini oluşturur:

* [**Azure IOT hub'ı**](https://azure.microsoft.com/services/iot-hub/): Bulut cihaz için bağlanan ağ geçidi. Hub ve işlem çok geniş hacimlerdeki verileri başına bağlantı milyonlarca cihaz başına kimlik doğrulama desteği, çözümünüzün güvenliğini sağlamanıza yardımcı ölçeklendirebilirsiniz.

* [**Azure Cosmos DB**](https://azure.microsoft.com/services/cosmos-db/): Meta veri öznitelikleri, yapılandırma ve güvenlik özellikleri gibi sağlamak cihazların yöneten yarı yapılandırılmış veriler için ölçeklenebilir, tam olarak dizini oluşturulan bir veritabanı hizmeti. Azure Cosmos DB, yüksek performanslı ve yüksek performanslı işleme, veri ve zengin bir SQL sorgusu arabirimi şemadan dizin sunar.

* [**Azure Stream Analytics**](https://azure.microsoft.com/services/stream-analytics/): Gerçek Zamanlı Akış sayesinde hızlı bir şekilde geliştirin ve cihazlar, algılayıcılar, altyapı ve uygulamalardan gerçek zamanlı Öngörüler açığa çıkarmak için düşük maliyetli bir analiz çözümü dağıtmak bulutta işleme. Bu tümüyle yönetilen bu hizmet verileri, herhangi bir birim için yine de yüksek aktarım hızı, düşük gecikme süresi ve dayanıklılık elde edin ölçeklendirebilirsiniz.

* [**Azure uygulama hizmetleri**](https://azure.microsoft.com/services/app-service/): Güçlü web uygulamaları ve her yerden veri bağlanan mobil uygulamaları oluşturmak için bir bulut platformu; bulutta veya şirket içinde. iOS, Android ve Windows için ilgi çekici mobil uygulamalar oluşturun. Yazılım olarak hizmet (SaaS) ve onlarca bulut tabanlı hizmetler için kullanıma hazır bağlantısı ile Kurumsal uygulamalar ve kurumsal uygulamaları ile tümleştirin. En sevdiğiniz dilde ve IDE'de kodlamaya — .NET, Node.js, PHP, Python veya Java — için web uygulamaları ve API'leri her zamankinden daha hızlı oluşturun.

* [**Logic Apps**](https://azure.microsoft.com/services/app-service/logic/): Azure App Service'in Logic Apps özelliği, IOT çözümünüzün var olan satır iş kolu sistemlerinize tümleştirin ve iş akışı işlemlerini otomatikleştirmek yardımcı olur. Logic Apps, geliştiricilerin bir tetikleyiciyle başlatılan ve bir dizi adım yürüten iş akışları tasarlamasına olanak tanır; kurallar ve İş süreçlerinizi ile tümleştirmek için güçlü bağlayıcıları kullanma eylemler. Logic Apps geniş bir SaaS, bulut tabanlı ekosistemine kullanıma hazır bağlantısı sunar ve şirket içi uygulamaları.

* [**Azure Blob Depolama**](https://azure.microsoft.com/services/storage/): Cihazlarınızı buluta gönderdiğiniz verileri için güvenilir, ekonomik bulut depolama.