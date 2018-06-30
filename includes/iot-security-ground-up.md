---
title: include dosyası
description: include dosyası
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: b3525f461d0662db5bf3677f7e981bbdbc663d50
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37103652"
---
# <a name="internet-of-things-security-from-the-ground-up"></a>Nesnelerin interneti güvenlik sıfırdan

Nesnelerin interneti (IOT) işletmelere dünya çapındaki benzersiz güvenlik, gizlilik ve uyumluluk sorunları doğurur. Bu sorunları yazılım ve nasıl uygulandığı burada Uzayda Döndür geleneksel siber teknolojisi farklı olarak, sanal ve fiziksel dünyaları yakınsama ne olur IOT ilgilidir. IOT çözümleri koruma cihazları, bu cihazlar ve Bulut ve işleme ve depolama sırasında bulutta güvenli veri koruması arasında güvenli bağlantı güvenli sağlanmasını sağlama gerektirir. Bu tür işlevselliği karşı çalışan, ancak, kaynak kısıtlı cihazları, dağıtımları coğrafi dağıtılması ve aygıtları bir çözüm içinde çok sayıda değildir.

Bu makalede IOT Çözüm Hızlandırıcıları güvenli ve özel bir nesnelerin interneti bulut çözümünün nasıl sağladığını açıklar. Çözüm Hızlandırıcıları eksiksiz bir uçtan uca çözüm sıfırdan her aşamada yerleşik güvenlik sunar. Microsoft, güvenli yazılım geliştirme Microsoft'un on yılları kökü yazılım mühendislik uygulama parçası olan uzun deneyimi güvenli yazılım geliştirme. Bu, güvenlik geliştirme yaşam döngüsü (SDL) bir ana bilgisayar, işletimsel güvenlik güvencesi (OSA) ve Microsoft dijital Suçlar Birimi'nin, Microsoft gibi bir altyapı düzeyinde güvenlik hizmetleri ile birlikte temel geliştirme metodolojisini sağlamaktır Güvenlik Yanıt Merkezi ve Microsoft Malware Protection Center.

Sağlama yapma, bağlanma ve IOT aygıtlardan kolay ve saydam veri depolama çözümü Hızlandırıcıları benzersiz özellikler sunar ve en önemlisi, güvenli. Bu makale Azure IOT Çözüm Hızlandırıcıları güvenlik özelliklerini inceler ve güvenlik, gizlilik ve uyumluluk sorunları emin olmak için dağıtım stratejilerini ele alınmıştır.

## <a name="introduction"></a>Giriş

Nesnelerin interneti (IOT) maliyetlerini azaltmak, geliri artırmak ve kendi iş dönüştürmek için gerçek fırsatlar ve gelecekteki, işletmelerin hemen sunumu wave ' dir. Birçok işletme, ancak bunların kuruluşlardaki güvenlik, gizlilik ve uyumluluk endişeniz nedeniyle IOT dağıtmak şüpheli. Önemli önemli bir nokta siber ve fiziksel dünyaları birlikte, bu iki dünyaları devralınan tek tek risklerin bileşik birleştirir IOT altyapı benzersizliğini gelir. Güvenlik IOT ilgili cihazlarda çalışan kod bütünlüğünü sağlamak için aygıt ve kullanıcı kimlik doğrulaması sağlayarak, cihazları (yanı sıra bu cihazları tarafından oluşturulan veri) Temizle sahipliğini tanımlama ve siber ve fiziksel saldırılara esnek.

Ardından, gizlilik sorun yoktur. Şirketler, erişim ve benzeri denetleyen görebilen ne toplanacak gibi ve neden, veri toplama, ilgili saydamlık istiyorsunuz. Son olarak, bunları işletim kişiler birlikte ekipmanının genel güvenlik sorunlarını ve endüstri standartları uyumluluk sürdürme sorunları vardır.

Verilen güvenlik, gizlilik, saydamlık ve uyumluluk sorunları, doğru IOT çözümü sağlayıcısı seçme, bir sınama kalır. IOT yazılım ve hizmetlerinin çeşitli satıcılar tarafından sağlanan tek tek parçaları birlikte Dikiş güvenlik, gizlilik, saydamlık ve algılamak, let alone düzeltmek sabit uyumluluk boşlukları tanıtır. Yazılım ve hizmet sağlayıcı verticals ve coğrafyalara yayılan, ancak aynı zamanda güvenli ve saydam bir şekilde ölçeklendirebilirsiniz Hizmetleri çalıştıran geniş kapsamlı deneyimlerden sahip sağlayıcıları bulunması temel sağ IOT seçeneği. Benzer şekilde, bu makineler dünya çapında bir milyarlarca üzerinde çalışan güvenli yazılım geliştirme ile deneyiminin on yılları olması ve bu nesnelerin interneti yeni world tarafından teşkil tehdit için teşekkür ederiz olanağına sahip seçili sağlayıcı için yardımcı olur.

## <a name="secure-infrastructure-from-the-ground-up"></a>Sıfırdan güvenli altyapısı

[Microsoft Cloud](https://azure.microsoft.com) altyapısını 127 ülkede bir milyardan fazla müşteriler destekler. Kurumsal Yazılım oluşturma ve dünyanın en büyük çevrimiçi hizmetlerin bazıları çalıştırma Microsoft'un on yılları uzun deneyimi üzerinde çizim, Microsoft Cloud Artırılmış güvenlik, gizlilik, uyumluluk ve tehdit daha yüksek düzeyde azaltma yöntemler sağlar ' den müşterilerin çoğu, kendi elde.

[Güvenlik geliştirme yaşam döngüsü (SDL)](https://www.microsoft.com/sdl/) güvenlik gereksinimlerini tüm yazılım yaşam döngüsü içine katıştırır zorunlu şirket çapında geliştirme işlemi sunar. İşletimsel etkinlikler aynı düzeyde güvenlik uygulamalarını izleyin sağlamaya yardımcı olmak için Microsoft'un işletimsel güvenlik güvencesi (OSA) işleminde düzenlendiği sıkı güvenlik yönergeleri SDL kullanır. Microsoft, üçüncü taraf denetim firmalarından uyumluluk yükümlülüklerin karşıladığından ve Microsoft Geniş güvenlik çaba mükemmel, Microsoft dijital Suçlar Birimi'nin dahil olmak üzere merkezlerinin oluşturulmasını aracılığıyla ilgilenir. devam eden doğrulama ile de çalışır, Microsoft Güvenlik Yanıt Merkezi ve Microsoft Malware Protection Center.

## <a name="microsoft-azure---secure-iot-infrastructure-for-your-business"></a>Microsoft Azure - işletmeniz için güvenli IOT altyapısı

Microsoft Azure tam bulut çözüm, sürekli büyüyen bir tümleşik bulut hizmetler koleksiyonu birleştiren bir sunar — analizi, machine learning, depolama, güvenlik, ağ ve web — bir endüstri lideri taahhüt koruma ile ve verilerinizin. Microsoft'un [ihlali varsayın](https://azure.microsoft.com/blog/red-teaming-using-cutting-edge-threat-simulation-to-harden-the-microsoft-enterprise-cloud/) stratejisi kullanan ayrılmış bir *kırmızı takım* saldırı algılama yeteneği Azure test benzetimini yazılım güvenlik uzmanlar, ortaya çıkan tehditlere karşı korumak ve kurtarmak ihlallerinden. Microsoft'un [genel olay yanıtlama](https://www.microsoft.com/TrustCenter/Security/DesignOpSecurity) aralıksız saldırıları ve kötü amaçlı etkinliği etkilerini azaltmak için takım çalışır. Takım olay yönetimini, iletişim ve kurtarma için kurulmuş yordamları izler ve iç ve dış iş ortaklarıyla bulunabilir ve tahmin edilebilir arabirimleri kullanır.

Microsoft'un sistemleri sürekli izinsiz giriş algılama ve önleme, hizmet saldırı önleme, normal sızma test ve tanımlamak ve tehditlerin azaltılmasına yardımcı adli araçlar sağlar. [Çok faktörlü kimlik doğrulaması](../articles/active-directory/authentication/multi-factor-authentication.md) fazladan bir ağa erişmek son kullanıcılar için güvenlik katmanı sağlar. Ve Microsoft uygulaması ve ana bilgisayar sağlayıcısı için erişim denetimi, izleme, kötü amaçlı yazılım, güvenlik açığı taraması, düzeltme ekleri ve yapılandırma yönetimi sunar.

Çözüm Hızlandırıcıları için güvenlik ve gizlilik Azure platformu SDL ve OSA işlemleri için güvenli geliştirme ve tüm Microsoft yazılım işlemi ile birlikte yerleşik yararlanın. Bu yordamları altyapı koruma, ağ koruması ve herhangi bir çözüm güvenlik için temel kimlik ve yönetim özellikleri sağlar.

[Azure IOT Hub](../articles/iot-hub/iot-hub-what-is-iot-hub.md) içinde [IOT Çözüm Hızlandırıcıları](../articles/iot-accelerators/iot-accelerators-what-is-azure-iot.md) gibi IOT cihazları ve Azure hizmetleri arasında güvenilir ve güvenli çift yönlü iletişim sağlayan tam olarak yönetilen bir hizmet sunar [Azure Machine Learning](../articles/machine-learning/studio/what-is-machine-learning.md) ve [Azure akış analizi](../articles/stream-analytics/stream-analytics-introduction.md) cihaz başına güvenlik kimlik ve erişim denetimi kullanarak.

En iyi güvenlik ve gizlilik özellikleri Azure IOT Çözüm Hızlandırıcıları yerleşik iletişim kurmak için bu makalede üç birincil güvenlik alanları pakete aşağı keser.

![Azure IoT çözüm hızlandırıcıları](media/iot-security-ground-up/securing-iot-ground-up-fig3.png)

### <a name="secure-device-provisioning-and-authentication"></a>Güvenli aygıt sağlama ve kimlik doğrulama

Out alanında IOT altyapısı tarafından çalışıyorken aygıtla iletişim kurmak için kullanılan her aygıt için benzersiz kimlik anahtarı sağlayarak oldukları sırada Çözüm Hızlandırıcıları aygıtları güvenliğini sağlayın. Hızlı ve kolay kurulum işlemidir. Oluşturulan anahtarı bir kullanıcı tarafından seçilen cihaz kimliği ile Azure IOT hub'ı ile cihaz arasındaki tüm iletişimi kullanılan belirtecin temelini oluşturur.

Cihaz kimlikleri (yani bir donanım güven modülünde Flash yazma ile yüklenen olan) üretim sırasında bir aygıtla ilişkili olabilir veya mevcut bir proxy (örneğin CPU seri numaraları) kimlik sabit kullanabilirsiniz. Bu tanımlayıcı bilgileri aygıtın değiştirme basit olmadığından, temel alınan aygıt Donanım değişikliklerini ancak mantıksal aygıt kalır durumda aynı mantıksal aygıt kimlikleri tanıtmak önemlidir. Bazı durumlarda, bir cihaz kimliği ilişkisini aygıt dağıtım sırasında olabilir (örneğin, kimliği doğrulanmış alan mühendisin fiziksel olarak yeni bir cihaz çözüm arka ucuyla iletişim kurulurken yapılandırır). [Azure IOT Hub kimlik kayıt defteri](../articles/iot-hub/iot-hub-devguide.md) cihaz kimliklerini ve güvenlik anahtarlarını bir çözüm için güvenli olarak depolanmasını sağlar. Tek tek veya grup cihaz kimlikleri bir izin verilenler listesi veya Engellenenler listesi, cihaz erişimi üzerinde tam denetimi etkinleştirme eklenebilir.

Bulutta Azure IOT hub'ı erişim denetimi ilkeleri etkinleştirme ve gerektiğinde bir IOT dağıtım aygıttan ilişkisini kaldırmak için bir yol sağlayarak, herhangi bir cihaz kimliği devre dışı bırakma etkinleştirin. Bu ilişkilendirme ve aygıtların disassociation her bir cihaz kimliği dayanır.

Ek aygıt güvenlik özellikleri içerir:

* Cihazlar istenmemiş ağ bağlantıları kabul etmez. Bunlar, yalnızca giden bir şekilde tüm bağlantıları ve yolları oluşturun. Bir komutu arka ucundan almak bir cihaz için cihaz işlemek tüm bekleyen komut olup olmadığını denetlemek için bir bağlantı başlatması gerekir. Sonra buluttan cihaza Mesajlaşma, aygıt ve IOT hub'ı arasında bir bağlantı güvenli bir şekilde kurulur ve bulut cihaza saydam olarak gönderilebilir.
* Aygıtları yalnızca bağlanmak veya iyi bilinen hizmetleri ile kullanıcılar, Azure IOT Hub gibi eşlendikleri yolları oluşturun.
* Sistem düzeyinde yetkilendirme ve kimlik doğrulama cihaz başına kimliği, erişim kimlik bilgileri ve izinleri neredeyse yapmadan kullanın-anında iptal edilebilir.

### <a name="secure-connectivity"></a>Güvenli bağlantı

Mesajlaşma dayanıklılık, tüm IOT çözümünün önemli bir özelliktir. İşlemi komutları teslim ve/veya aygıtlardan veri almak için gereken IOT cihazlarının Internet veya güvenilmeyen benzer diğer ağlar üzerinden bağlı olarak çizilir. Azure IOT Hub bulut ile ilgili kaynaklar iletilere yanıt olarak bir sistem aracılığıyla aygıtları arasında Mesajlaşma dayanıklılık sunar. İleti için ek dayanıklılık yedi güne kadar telemetri ve komutlar için iki gün için IOT hub'ı iletilerinde önbelleğe alarak elde edilir.

Verimlilik, kaynakların ve kaynak kısıtlı bir ortamda işlemi koruma sağlamak önemlidir. HTTPS (güvenli), HTTP sık kullanılan http protokolü, endüstri standardı güvenli sürümü verimli iletişimi etkinleştirme Azure IOT Hub tarafından desteklenir. Gelişmiş Message Queuing Protokolü (AMQP) ve Message Queuing Telemetri Aktarım (Azure IOT Hub tarafından desteklenen MQTT), yalnızca kaynak kullanımının aynı zamanda güvenilir ileti teslimi açısından verimliliği için tasarlanmıştır. 

Ölçeklenebilirlik özelliği çok çeşitli aygıtları güvenli bir şekilde çalışmasını gerektirir. Azure IOT hub IP etkin hem IP etkin olmayan cihazlara güvenli bağlantı sağlar. IP özellikli cihazların doğrudan bağlanmak ve IOT Hub ile güvenli bir bağlantı üzerinden iletişim kurmak kullanabilirsiniz. Cihazlar olmayan IP etkin kaynak kısıtlı ve yalnızca Zwave, ZigBee ve Bluetooth gibi kısa uzaklığı iletişim protokolleri üzerinden bağlanabilir. Bir alan ağ geçidi bulut ile güvenli çift yönlü iletişim sağlamak için protokol çevirisi gerçekleştirir ve bu aygıtları toplamak için kullanılır.

Ek bağlantı güvenlik özellikleri içerir:

* Cihazlar ve Azure IOT hub'ı arasında veya ağ geçitleri ve Azure IOT Hub, iletişim yolunun X.509 protokolünü kullanarak kimlik doğrulaması Azure IOT Hub ile endüstri standardı Aktarım Katmanı Güvenliği (TLS) kullanarak güvenli hale getirilir.
* Gelen istekte bulunulmamış gelen bağlantıları cihazları korumak için Azure IOT Hub cihaz herhangi bir bağlantı açılmaz. Cihaz tüm bağlantıları başlatır.
* Azure IOT Hub işlemi iletileri cihazlar için depolar ve cihazın bağlanmak için bekler. Bu komutlar, düzensiz aralıklarla hataya, bu komutları almak için güç veya bağlantı sorunları nedeniyle, bağlanan cihazları etkinleştirme iki gün süreyle depolanır. Azure IOT Hub cihaz başına kuyruk her aygıt için tutar.

### <a name="secure-processing-and-storage-in-the-cloud"></a>İşleme ve depolama bulutta güvenli

Şifrelenmiş iletişimlerinden bulutta veriler işlenirken Çözüm Hızlandırıcıları verilerinin güvende tutulmasına yardımcı olur. Ek şifreleme ve güvenlik anahtar yönetimi uygulamak için esneklik sağlar.

Kullanıcı kimlik doğrulaması ve yetkilendirme için Azure Active Directory (AAD) kullanarak Azure IOT Çözüm Hızlandırıcıları bulutta, denetlenen ve gözden kolay erişim yönetimini etkinleştirme verileri için bir ilke tabanlı bir yetkilendirme modeli sağlayabilir. Bu model, ayrıca neredeyse anında iptal bulut veri erişimi ve Azure IOT Çözüm Hızlandırıcıları için bağlı aygıtlar sağlar.

Verileri bulutta eklendiğinde, işlenen ve hiçbir kullanıcı tarafından tanımlanan iş akışı içinde depolanır. Her veri parçası için erişim Azure Active Directory ile kullanılan depolama hizmeti bağlı olarak denetlenir.

IOT altyapısı tarafından kullanılan tüm anahtarları anahtarları sağlama gerekebileceğinden geçir olanağı güvenli depolama bulutta depolanır. Veri depolanabilir [Azure Cosmos DB](../articles/cosmos-db/introduction.md) veya [SQL veritabanları](../articles/sql-database/sql-database-faq.md), istenen güvenlik düzeyini tanımını etkinleştirme. Ayrıca, Azure izlemek ve verilerinize herhangi yetkisiz erişim, sizi uyarmak için tüm erişim ya da yetkisiz erişimi denetlemek için bir yol sağlar.

## <a name="conclusion"></a>Sonuç

Nesnelerin interneti, olguları başlatır — işletmeler için en önemli noktalar. IOT inanılmaz değeri maliyetlerini azaltma gelir artırma ve iş dönüştürme için bir iş sunabilir. Bu dönüşüm başarısını büyük ölçüde sağ IOT yazılım ve hizmet sağlayıcısı seçme bağlıdır. Yalnızca bu dönüşüm anlama iş gereksinimlerine göre catalyzes, ancak Ayrıca, hizmetleri ve güvenlik, gizlilik, saydamlık ve önemli tasarım konuları olarak uyum ile oluşturulan yazılım sağlar bir sağlayıcı bulma anlamına gelir. Microsoft geliştirme ve güvenli yazılım ve Hizmetleri dağıtma ile geniş kapsamlı deneyimlerden ve öncü bu yeni nesnelerin interneti geçerlilik süresi içinde olacak şekilde devam eder.

Çözüm Hızlandırıcıları işletmeler dönüştürmek için güvenli, verimliliklerini varlıklar izlemeyi etkinleştirme tasarım gereği, sürücü işletimsel performansı yenilik etkinleştirmek ve gelişmiş veri analizi kullanımlar için güvenlik önlemleri oluşturun. Güvenlik, birden çok güvenlik özellikleri ve tasarım desenleri doğrultusunda, katmanlı yaklaşımın ile Çözüm Hızlandırıcıları herhangi bir işletme dönüştürmek için güvenilen bir altyapı dağıtımına yardımcı olur.

## <a name="additional-information"></a>Ek bilgiler

Her Çözüm Hızlandırıcısı gibi Azure hizmetlerini örneklerini oluşturur:

* [**Azure IOT Hub**](https://azure.microsoft.com/services/iot-hub/): Bulut aygıtına bağlayan ağ geçidiniz. Hub ve işlem büyük miktarda veriyi başına bağlantı milyonlarca cihaz başına kimlik doğrulama desteği çözümünüzün güvenliğini sağlamanıza yardımcı ölçeklendirebilirsiniz.
* [**Azure Cosmos DB**](https://azure.microsoft.com/services/cosmos-db/): cihazlar için meta verileri yönetir yarı yapılandırılmış veriler için ölçeklenebilir, tam olarak dizine veritabanı hizmeti, öznitelikler, yapılandırma ve güvenlik özellikleri gibi sağlamanız. Azure Cosmos DB yüksek performanslı ve yüksek verimlilik işleme, veri ve zengin bir SQL sorgu arabirimi şema belirsiz dizin sunar.
* [**Azure Stream Analytics**](https://azure.microsoft.com/services/stream-analytics/): Gerçek Zamanlı Akış hızlı bir şekilde geliştirmek ve cihazlar, algılayıcılar, altyapı ve uygulamalardan gerçek zamanlı Öngörüler ortaya çıkarmak için düşük maliyetli analiz çözümü dağıtmak sağlayan bulutta işleme . Bu tam olarak yönetilen hizmet verilerden herhangi bir birime yüksek performans, düşük gecikme süresi ve dayanıklılık hala elde ederken ölçeklendirebilirsiniz.
* [**Azure uygulama hizmetleri**](https://azure.microsoft.com/services/app-service/): güçlü web ve bağlanmak için herhangi bir yerde veri; bulutta veya şirket içi mobil uygulamaları oluşturmak için bir bulut platformu. iOS, Android ve Windows için ilgi çekici mobil uygulamalar oluşturun. Hizmet (SaaS) ve bulut tabanlı Hizmetleri düzinelerce Giden kutusu bağlantı Kurumsal uygulamaları ve kuruluş uygulamaları olarak, yazılım ile tümleştirin. Sık kullanılan dil ve IDE kodda — .NET, Node.js, PHP, Python veya Java — her zamankinden daha hızlı web uygulamaları ve API'ları oluşturmak için.
* [**Logic Apps**](https://azure.microsoft.com/services/app-service/logic/): Azure App Service Logic Apps özelliğini IOT çözümünüzü mevcut iş kolu satır sistemlerinize tümleştirmek ve iş akışı işlemlerini otomatikleştirmenize yardımcı olur. Logic Apps, geliştiricilerin; bir tetikleyiciyle başlayan ve bir dizi adımı yürütürken iş akışları tasarlamasına sağlar — kurallar ve Eylemler, iş süreçlerini tümleştirmek için güçlü bağlayıcıları kullanın. Logic Apps SaaS, bulut tabanlı büyük ekosistemi Giden kutusu bağlantısını sunar ve şirket içi uygulamalar.
* [**Azure blob depolama**](https://azure.microsoft.com/services/storage/): aygıtlarınızı buluta Gönder verileri için güvenilir ve ekonomik bulut depolama.