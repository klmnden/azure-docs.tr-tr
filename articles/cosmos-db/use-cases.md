---
title: Yaygın kullanım örnekleri ve senaryoları için Azure Cosmos DB
description: 'Azure Cosmos DB için beş kullanım örnekleri hakkında üst bilgi: kullanıcı oluşturulan içerik, olay günlüğü, veri Kataloğu, kullanıcı tercihlerini veri ve nesnelerin interneti (IOT).'
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.topic: conceptual
ms.date: 05/21/2019
ms.openlocfilehash: 28a4cc854842b66a9fb61134e3ca9ac9a5f38fed
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65965598"
---
# <a name="common-azure-cosmos-db-use-cases"></a>Yaygın Azure Cosmos DB kullanım örnekleri
Bu makalede, Azure Cosmos DB için bazı ortak kullanım durumları için genel bir bakış sağlar.  Cosmos DB ile Uygulamanızı geliştirirken bu makaledeki önerileri bir başlangıç noktası olarak hizmet eder.   

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlamak mümkün olacaktır: 

* Azure Cosmos DB için yaygın kullanım örnekleri nelerdir?
* Perakende uygulamaları için Azure Cosmos DB kullanmanın avantajları nelerdir?
* Azure Cosmos DB, nesnelerin interneti (IOT) sistemleri için bir veri deposu olarak kullanmanın avantajları nelerdir?
* Web ve mobil uygulamaları için Azure Cosmos DB kullanmanın avantajları nelerdir?

## <a name="introduction"></a>Giriş
[Azure Cosmos DB](../cosmos-db/introduction.md) Microsoft'un Global olarak dağıtılmış veritabanı hizmetidir. Hizmet, esnek bir biçimde (ve birbirinden bağımsız olarak) herhangi sayıda coğrafi bölgesinde aktarım hızını ve depolamayı ölçeklendirme müşterilerin izin vermek için tasarlanmıştır. Azure Cosmos DB, ilk Global olarak dağıtılmış veritabanı hizmeti pazarında bugün kapsamlı sunmaya [hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/cosmos-db/) aktarım hızı, gecikme süresi, kullanılabilirlik ve tutarlılık. 

Azure Cosmos DB çok çeşitli uygulamalar ve kullanım örnekleri kullanılan bir genel dağıtılmış, çok modelli veritabanıdır. Herhangi bir için iyi bir seçenek olup [sunucusuz](https://azure.com/serverless) gereken milisaniye sipariş düşük yanıt süreleri ve hızlı bir şekilde ve küresel olarak ölçeklemek gereken uygulama. Birçok veri modelini destekler (anahtar-değer, belgeler, grafikler ve sütunlu) ve veriler için birçok API'lere erişim de dahil olmak üzere [Azure Cosmos DB'nin MongoDB API'si](mongodb-introduction.md), [SQL API](documentdb-introduction.md), [Gremlin API](graph-introduction.md), ve [tablolar API'SİNİN](table-introduction.md) yerel olarak ve Genişletilebilir bir şekilde. 

Azure Cosmos DB'nin genel kullanımına açma hedefimizde sahip yüksek performanslı uygulamalar için uygun hale bazı öznitelikleri şunlardır:

* Azure Cosmos DB, verilerinizi yüksek kullanılabilirlik ve ölçeklenebilirlik için yerel olarak bölümler. Azure Cosmos DB kullanılabilirlik, aktarım hızı, düşük gecikme süresi ve tutarlılık tutarlılıkla tek bölgeli tüm hesaplar ve çok bölgeli tüm hesaplar için % 99,99 oranında garanti sunar ve üzerinde çok bölgeli tüm veritabanı hesaplarında % 99,999 okunabilirlik.
* Azure Cosmos DB, düşük gecikme süreli milisaniye sipariş yanıt süresi, SSD destekli depolama sahiptir.
* Tam esneklik ve düşük maliyet / performans oranı için nihai, tutarlı ön ek, oturum ve sınırlanmış eskime tutarlılığı gibi tutarlılık düzeyleri için Azure Cosmos DB'nin desteği sağlar. Hiçbir veritabanı hizmeti olarak Azure Cosmos DB kadar esneklik düzeyleri tutarlılık sunar. 
* Azure Cosmos DB, depolamayı ve aktarım hızını bağımsız olarak ölçümleri bir esnek veri kullanımı kolay fiyatlandırma modeline sahiptir.
* Azure Cosmos DB'nin ayrılmış aktarım hızı modeli, CPU/bellek/IOPS temel alınan donanım yerine okuma/yazma sayısı bakımından düşünme olanak tanır.
* Çok büyük bir istek birimlerine günlük trilyonlarca sırasına göre ölçeği azure Cosmos DB'nin tasarım olanak tanır.

Bu öznitelikler faydalı web, mobil, oyun ve IOT uygulamaları düşük yanıt süreleri gerekir ve okuma ve yazma işlemleri büyük miktarda ele almanız gerekir.

## <a name="iot-and-telematics"></a>IoT ve telematikler
IOT kullanım örnekleri, yaygın olarak nasıl bunlar, işlem, içe alma, bazı desen ve veri depolayın.  İlk olarak, bu sistemler çeşitli gezinin cihaz sensörlerden alınan verilerin ani artışlara alabilen gerekir. Ardından, bu sistemler işleyebilir ve gerçek zamanlı Öngörüler akış verilerini analiz edin. Verileri toplu analiz için soğuk depolama için ardından arşivlenir. Microsoft Azure IOT için uygulanabilir zengin Hizmetleri Azure Cosmos DB, Azure Event Hubs, Azure Stream Analytics, Azure bildirim hub'ı, Azure Machine Learning, Azure HDInsight ve Power BI dahil olmak üzere kullanım sunar. 

![Azure Cosmos DB IOT başvuru mimarisi](./media/use-cases/iot.png)

Düşük gecikme süresi ile yüksek aktarım hızı veri alımı sunduğu veri ani artışlara Azure olay hub'ları tarafından aktarılabilir. İçin gerçek zamanlı öngörülere işlenmesi gereken alınan verileri gerçek zamanlı analiz için Azure Stream Analytics'e funneled. Geçici sorgulama için Azure Cosmos DB'ye veri yüklenebilir. Azure Cosmos DB'ye veriler yüklendikten sonra veri sorgulanacağı hazırdır. Ayrıca, yeni veri ve mevcut verilerde yapılan değişiklikleri değişiklik akışı üzerinde okunabilir. Değişiklik akışı kalıcı bir ise, Cosmos DB kapsayıcıları yapılan sıralı bir düzende depolayan günlük ekleme. Tüm veriler veya yalnızca Azure Cosmos DB'de veri değişiklikleri, başvuru verileri gerçek zamanlı analiz parçası olarak kullanılabilir. Ayrıca, veriler daha fazla iyileştirilmektedir ve Azure Cosmos DB veri için HDInsight Pig, Hive veya Map/Reduce işleri bağlanarak işlenen.  Daraltılmış veri Azure Cosmos DB geri yüklenir ve raporlama için.   

Azure Cosmos DB, EventHubs ve Storm kullanarak örnek bir IOT çözüm için bkz: [hdınsight storm örnekleri GitHub deposunda](https://github.com/hdinsight/hdinsight-storm-examples/).

IOT için Azure teklifleri hakkında daha fazla bilgi için bkz. [kendi nesnelerin interneti oluşturma](https://www.microsoft.com/en-us/internet-of-things). 

## <a name="retail-and-marketing"></a>Perakende ve pazarlama
Azure Cosmos DB, Windows Store ve XBox Live çalıştıran Microsoft'un kendi e-ticaret platformları, yaygın olarak kullanılır. Olay kaynağını belirleme sipariş işleme komut zincirlerini ve kataloğu verilerini depolamak için perakende sektöründe de kullanılır.

Kataloğu veri kullanım senaryoları, depolama ve sorgulama kişiler, yerler ve ürünleri gibi varlıklar için bir dizi içerir. Bazı Kataloğu veri kullanıcı hesapları, ürün kataloglarını, IOT cihazı kayıt defterlerini ve fatura malzemeleri sistemlerinin örnekleridir. Bu veriler için öznitelikler değişebilir ve zaman içinde uygulama gereksinimlerine uyacak şekilde değiştirebilirsiniz.

Bir ürün kataloğu örneği için bir otomotiv parçaları tedarikçi göz önünde bulundurun. Her bölüm kendi öznitelikleri paylaşan tüm bölümleri ortak öznitelikleri yanı sıra olabilir. Ayrıca, özel bir bölümü için öznitelikler, yeni bir modeli yayınlandığında aşağıdaki yıl değiştirebilirsiniz. Azure Cosmos DB, esnek şemalar ve hiyerarşik veri destekler ve bu nedenle, ürün kataloğu verilerini depolamak için uygundur.

![Azure Cosmos DB perakende Kataloğu başvuru mimarisi](./media/use-cases/product-catalog.png)

Azure Cosmos DB, için güç aktivita typu EventDriven mimarileri kullanarak olay kaynağını belirleme için sık sık kullanılır, [değişiklik akışını](change-feed.md) işlevselliği. Değişiklik akışı, aşağı akış mikro Hizmetleri güvenilir bir şekilde ve artımlı olarak ekler ve bir Azure Cosmos DB için yapılan güncelleştirmeler (örneğin, sipariş olayları) okuma yeteneği sağlar. Durum değiştirme olayları ve birçok mikro hizmetler arasında sürücü sipariş işleme iş akışı kalıcı olay deposuna bir ileti aracısı olarak sağlamak için bu işlevi yararlanılabilir (hangi uygulanabilir olarak [sunucusuz Azure işlevleri](https://azure.com/serverless)).

![Azure Cosmos DB ile işlem hattı başvuru mimarisi sıralama](./media/use-cases/event-sourcing.png)

Ayrıca, Apache Spark işleri aracılığıyla büyük veri analizi için HDInsight ile Azure Cosmos DB'de depolanan verileri tümleştirilebilir. Azure Cosmos DB için Spark Bağlayıcısı hakkında daha fazla bilgi için bkz: [HDInsight ve Cosmos DB ile bir Spark işi çalıştırma](spark-connector.md).

## <a name="gaming"></a>Oyun
Veritabanı katmanı, oyun uygulamaları, önemli bir bileşenidir. Modern oyunlar grafik işleme mobile/konsol istemcilerde gerçekleştirir, ancak bulutta oyun içi istatistikler, sosyal medya tümleştirmesine ve yüksek puan tabloları gibi özelleştirilmiş ve kişiselleştirilmiş içerik sağlamak için kullanır. Oyunlar genellikle okuma için tek milisaniyelik gecikme süreleri gerektiren ve oyun içi deneyimi bir ilgi çekici sağlamak için yazar. Oyun veritabanını hızlı ve istek hızları büyük artış işleme sırasında yeni oyun başlatır ve özellik güncelleştirmeleri olması gerekir.

Azure Cosmos DB gibi oyunlar tarafından kullanılan [The Walking Dead: No Man's Land](https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) tarafından [sonraki oyunları](https://www.nextgames.com/), ve [Halo 5: Veli](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Azure Cosmos DB, Oyun geliştiriciler için aşağıdaki avantajları sağlar:

* Azure Cosmos DB performans ölçeklenmesine olanak sağlayan esnek bir şekilde aşağı veya yukarı. Bu, tek bir API çağrısı yaparak güncelleştirme profili ve düzinelerce gelen istatistikleri eşzamanlı oyuncular milyonlarca işlemek oyunlar sağlar.
* Azure Cosmos DB milisaniyelik okuma destekler ve oyun sırasında tüm aksamalar önlemeye yardımcı olmak yazar.
* Azure Cosmos DB'nin otomatik dizin oluşturma sağlayan gerçek zamanlı birden çok farklı özelliklere göre filtre uygulamak için örneğin, oyuncuların kendi iç player kimlikleri tarafından bulun veya kendi GameCenter, Facebook, Google kimlikleri veya sorgu tabanlı bir guild player üyeliği. Karmaşık dizin oluşturma veya parçalama altyapısı oluşturmaya gerek kalmadan bu mümkündür.
* Oyun içi sohbet iletileri, oynatıcı guild üyelikleri, tamamlanan zorlukları, yüksek puan tabloları ve sosyal grafikleri de dahil olmak üzere sosyal özelliklerini esnek bir şema ile uygulamak daha kolaydır.
* Bir yönetilen platform-bir hizmet olarak (PaaS) olarak Azure Cosmos DB, minimal kurulumu ve yönetimi için hızlı yineleme izin vermek için çalışma ve pazarlama süresini azaltmak gereklidir.

![Azure Cosmos DB oyun başvuru mimarisi](./media/use-cases/gaming.png)

## <a name="web-and-mobile-applications"></a>Web uygulamaları ve mobil uygulamalar
Azure Cosmos DB, web ve mobil uygulamaları içinde yaygın olarak kullanılır ve üçüncü taraf hizmetleri ile zengin, kişiselleştirilmiş deneyimler oluşturmak için tümleştirme sosyal etkileşimleri modellemek için oldukça uygundur. Cosmos DB SDK'ları kullanılan yapı zengin iOS ve Android uygulamaları popüler kullanarak olabilir [Xamarin framework](mobile-apps-with-xamarin.md).  

### <a name="social-applications"></a>Sosyal uygulamalar
Azure Cosmos DB için ortak kullanım durumu depolamak ve kullanıcı tarafından oluşturulan içerikler (UGC) sorgulamak için web, mobil ve sosyal medya uygulamalarını gösterir. Bazı UGC sohbet oturumları, tweetleri, blog gönderileri, derecelendirmeleri ve açıklamaları örnekleridir. Genellikle, sosyal medya uygulamalarda UGC serbest biçimli metin, özellikleri, etiketler ve katı yapısı tarafından sınırlanan değil ilişkileri hızının birleşimidir. Sohbetleri gibi içerik, yorumları ve gönderileri Cosmos DB'de dönüşümlerini veya karmaşık nesne ilişkisel eşleme katmanlara gerek kalmadan depolanabilir.  Veri özellikleri, eklenemez veya geliştiriciler uygulama kodu, bu nedenle hızlı geliştirme yükseltme yineleme gibi gereksinimlerini karşılamak için kolayca değiştirilemez.  

Üçüncü taraf sosyal ağlarla tümleştirme uygulamaları bu ağlardan şemaları değişen yanıt vermesi gerekir. Verileri Cosmos DB'de varsayılan olarak otomatik olarak dizine gibi verileri dilediğiniz zaman sorgulanması hazırdır. Bu nedenle, bu uygulamaları kendi gereksinimlerine göre projeksiyonlar alınacak esnekliğine sahipsiniz.

Sosyal uygulamalar birçoğu, global bir ölçekte alıştırmanızı ve öngörülemeyen kullanım düzenlerine gösteriyor. Veri deposunun ölçeklendirme esneklik, kullanım talebini eşleştirmek için uygulama katmanı olarak gereklidir.  Bir Cosmos DB hesabı altında ek veri bölümlerine ekleyerek ölçeği genişletebilirsiniz.  Ayrıca, birden çok bölgede başka bir Cosmos DB hesabı da oluşturabilirsiniz. Cosmos DB hizmetini bölge kullanılabilirliği için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/#services).

![Azure Cosmos DB web uygulaması başvuru mimarisi](./media/use-cases/apps-with-global-reach.png)

### <a name="personalization"></a>Kişiselleştirme
Günümüzde, modern uygulamalar, karmaşık görünümleri ve deneyimleri ile gelir. Kullanıcı tercihleri veya ruh yemek ve gereksinimlerini markalama genellikle dinamik şunlardır. Bu nedenle, uygulamaları etkili bir şekilde kullanıcı Arabirimi öğeleri ve deneyimlerini hızla oluşturmasına imkan kişiselleştirilmiş ayarları alabilmesi gerekir. 

JSON, Cosmos DB tarafından desteklenen bir biçim, yalnızca basit değildir ancak JavaScript tarafından kolayca yorumlanabilir UI düzen verilerini göstermek için etkili bir biçimidir. Cosmos DB, hızlı okuma yazma gecikme süresi düşük ile izin ver ayarlanabilir tutarlılık düzeyi sunar. Bu nedenle, Cosmos DB'de JSON belgeleri olarak kişiselleştirilmiş ayarları dahil olmak üzere kullanıcı Arabirimi düzen verilerini depolamak, kablo bu verileri almak için etkili bir anlamına gelir.

![Azure Cosmos DB web uygulaması başvuru mimarisi](./media/use-cases/personalization.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB'yi kullanmaya başlamak için izleyin bizim [hızlı başlangıçlar](create-sql-api-dotnet.md), bu yol, bir hesap ve Cosmos DB'yi kullanmaya başlama oluşturma işleminde. 

Veya daha fazla bilgi istiyorsanız, Cosmos DB kullanan müşteriler hakkında aşağıdaki müşteri hikayeleri kullanılabilir:

* [Jet.com](https://jet.com). E-ticaret girişimcisi, üst diken, Microsoft bulutunda çalışıyor, Cosmos DB global bir ölçekte yararlanır.
* [Asos.com'un](https://www.asos.com/). Asos.com'un bir İngiliz çevrimiçi moda ve koyabileceğiniz deposudur. Asos, öncelikle genç yetişkinlere yönelik, üzerinde 850 markalar ve bunun yanı sıra kendi dizi giysi ve Donatılar satar.
* [Toyota](https://www.toyota.com/). Toyota Motor Japonca bir otomotiv üreticisi bir şirkettir. Toyota, Cosmos DB küresel IOT uygulama için kullanılabilir.
* [Citrix](https://customers.microsoft.com/story/citrix). Azure Service Fabric ve Azure Cosmos DB'yi kullanarak çoklu oturum açma çözümü Citrix geliştirir
* [TEXA](https://customers.microsoft.com/story/texaspa) TEXA'ın Devrim niteliğindeki IOT çözüm vehicle sahipleri için saat, para, doğalgaz yardımcı olur — ve büyük olasılıkla yaşar.
* [Domino'nın Pizza](https://www.dominos.com). Domino'nın Pizza Inc. bir Amerikan pizza Restoran zinciri olur.
* [Johnson denetimleri](https://www.johnsoncontrols.com). Johnson denetimler, genel büyük teknoloji ve çok çeşitli müşteriler 150'den fazla ülkede/bölgede hizmet çok endüstriyel lider ' dir.
* [Microsoft Windows, Evrensel Store, Azure IOT Hub, Xbox Live ve diğer Internet ölçeğindeki Hizmetler](https://azure.microsoft.com/blog/how-azure-documentdb-planet-scale-nosql-helps-run-microsoft-s-own-businesses/). Nasıl Microsoft Azure Cosmos DB'yi kullanarak yüksek düzeyde ölçeklenebilir hizmetler oluşturur.
* [Microsoft Data ve analiz ekibi](https://customers.microsoft.com/story/microsoftdataandanalytics). Microsoft'un veri ve analiz ekibi, Azure Cosmos DB ile dünya ölçeğinde büyük veri koleksiyonu sağlar
* [Sulekha.com](https://customers.microsoft.com/story/sulekha-uses-azure-documentdb-to-connect-customers-and-businesses-across-india). Sulekha, müşteriler ve işletmeler Hindistan bağlanmak için Azure Cosmos DB kullanır.
* [NewOrbit](https://customers.microsoft.com/story/neworbit-takes-flight-with-azure-documentdb). Azure Cosmos DB ile uçuş NewOrbit alır.
* [Affinio](https://customers.microsoft.com/doclink/affinio-switches-from-aws-to-azure-documentdb-to-harness-social-data-at-scale). Affinio, sosyal verilerden uygun ölçekte yararlanmasına için Azure Cosmos DB için AWS'den geçer.
* [Sonraki oyunları](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). Kullanılmayan yürüyen: No Man's Land oyunu, Azure Cosmos DB tarafından desteklenen #1 numaraya yükseliyor.
* [Halo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Nasıl Halo 5, Azure Cosmos DB kullanarak sosyal oyun deneyimini uygulanır.
* [Cortana Analytics Galerisi](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana Analytics Galerisi - Azure Cosmos DB üzerinde oluşturulmuş ölçeklenebilir topluluk sitesi.
* [Meltem](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Entegratörü önde gelen esnek bulut teknolojileri birkaç dakika içinde çok uluslu firmaları genel bilgiler sağlar.
* [Haber Cumhuriyeti](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Katılımcı vatandaşlar için bilgi amaçlı ile haberi zeka ekleme. 
* [SGS International](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Dünya genelinde tutarlı rengini büyük markaların kartlarını SGS için etkinleştirin. Ve Azure'a SGS kapatır.
* [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Küresel lider Telenor hızına ile Bulutu kullanıyor. 
* [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). Deponun geleceğin hızlı arama ve kolay veri akışı üzerinde çalışır.
* [Nucleo](https://customers.microsoft.com/story/azure-based-software-platform-breaks-down-barriers-bet). Azure tabanlı bir yazılım platformu, işletmelere ve müşteriler arasındaki engelleri ayırır
* [Weka](https://customers.microsoft.com/story/weka-smart-fridge-improves-vaccine-management-so-more-people-can-be-protected-against-diseases). Weka akıllı Fridge daha fazla insanın hastalıklara karşı korunması için aşı yönetimini geliştirdi
* [Turuncu Tribes](https://customers.microsoft.com/story/theres-more-to-that-food-app-than-meets-the-eye-or-the-mouth). Gıda uygulamanın göz veya konuşurken karşılayıp daha çok daha fazlası vardır.
* [Real Madrid'in](https://customers.microsoft.com/story/real-madrid-brings-the-stadium-closer-to-450-million-f). Real Madrid stadyumu 450 milyon Microsoft Cloud ile ayağına getiriyor.
* [Tuku](https://customers.microsoft.com/story/tuku-makes-car-buying-fun-with-help-from-azure-services). Azure hizmetlerinin yardımıyla eğlenceli satın araba TUKU yapar
