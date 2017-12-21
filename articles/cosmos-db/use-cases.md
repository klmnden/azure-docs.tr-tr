---
title: "Ortak kullanım durumları ve Azure Cosmos DB senaryoları | Microsoft Docs"
description: "Beş kullanım için Azure Cosmos DB üst hakkında bilgi edinin: kullanıcı oluşturulan içeriği, olay günlüğü, katalog verilerini, kullanıcı tercihlerini veri ve nesnelerin interneti (IOT)."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: eca68a58-1a8c-4851-8cf8-6e4d2b889905
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mimig
ms.openlocfilehash: bcafc999c30d1e72971c8e26e951169ea6b56416
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="common-azure-cosmos-db-use-cases"></a>Ortak Azure Cosmos DB kullanım durumları
Bu makalede Azure Cosmos DB için birkaç ortak kullanım durumları genel bir bakış sağlar.  Bu makalede önerileri Cosmos DB ile Uygulamanızı geliştirirken bir başlangıç noktası olarak hizmet.   

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlayın mümkün olacaktır: 

* Azure Cosmos DB için ortak kullanım durumları nelerdir?
* Perakende uygulamaları için Azure Cosmos DB kullanmanın avantajları nelerdir?
* Nesnelerin interneti (IOT) sistemleri için bir veri deposu olarak Azure Cosmos DB kullanmanın avantajları nelerdir?
* Web ve mobil uygulamaları için Azure Cosmos DB kullanmanın avantajları nelerdir?

## <a name="introduction"></a>Giriş
[Azure Cosmos DB](../cosmos-db/introduction.md) Microsoft'un Genel dağıtılmış veritabanı hizmetidir. Hizmet özellikler esnek (ve bağımsız olarak) işleme ve depolama coğrafi bölgeler arasında herhangi bir sayı ölçeklendirme müşterilere izin verecek şekilde tasarlanmıştır. Azure Cosmos DB hizmettir ilk Genel dağıtılmış veritabanı pazardaki bugün kapsamlı sunmak için [hizmet düzeyi sözleşmelerine](https://azure.microsoft.com/support/legal/sla/cosmos-db/) verimlilik, gecikme, kullanılabilirlik ve tutarlılık. 

Azure Cosmos DB çok çeşitli uygulamalar ve kullanım örnekleri içinde kullanılan genel bir dağıtılmış, birden çok model veri tabanıdır. Herhangi bir için iyi bir seçimdir olan [sunucusuz](http://azure.com/serverless) düşük milisaniyelik sipariş yanıt sürelerini gerekir ve hızlı bir şekilde ve genel olarak ölçeklendirmek gereken uygulama. Birden çok veri modelleri destekler (anahtar-değer, belgeler, grafikler ve sütun) ve veriler için birçok API'lere erişim dahil olmak üzere [MongoDB API](mongodb-introduction.md), [SQL API](documentdb-introduction.md), [grafik API'si (Gremlin)](graph-introduction.md), ve [tabloları API](table-introduction.md) yerel olarak ve Genişletilebilir bir biçimde. 

Aşağıdaki genel ambition ile yüksek performanslı uygulamalar için oldukça uygun hale bazı Azure Cosmos DB öznitelikleridir.

* Azure Cosmos DB yerel olarak yüksek kullanılabilirlik ve ölçeklenebilirlik için verilerinizi bölümler. Azure Cosmos DB kullanılabilirlik, performans, düşük gecikme süresi ve esnek tutarlılık tutarlılığı tüm tek bölge ve tüm bölgeli hesapları için % 99,99 garantileri sağlar ve kullanılabilirlik tüm bölgeli veritabanı hesaplarda %99.999 okuyun.
* Azure Cosmos DB yedeklenen SSD depolama ile düşük gecikme süreli milisaniyelik sipariş yanıt sürelerini sahiptir.
* Tam esneklik ve düşük maliyet performans oranı için tutarlılık düzeylerini nihai, tutarlı öneki, oturum ve sınırlanmış eskime durumu gibi Azure Cosmos veritabanı desteği sağlar. Veritabanı Hizmet düzeyleri tutarlılık Azure Cosmos DB olarak kadar esnekliği sağlar. 
* Azure Cosmos DB bağımsız olarak depolama ve işleme ölçümler bir esnek veri kolay fiyatlandırma modeli vardır.
* Azure Cosmos DB'ın ayrılmış işleme modeli okuma/yazma CPU/bellek/IOPS temel alınan donanımın yerine sayısı bakımından düşünmek sağlar.
* Yoğun istek birimlerine günde istek trilyonlarca sırasına göre ölçeklendirme azure Cosmos DB'ın tasarım sağlar.

Bu öznitelikler faydalı web, mobil, oyun ve düşük yanıt sürelerini gerekir ve okuma ve yazma işlemleri oldukça büyük miktardaki işlemek gereken IOT uygulamaları.

## <a name="iot-and-telematics"></a>IoT ve telematikler
IOT kullanım örnekleri, yaygın olarak nasıl bunlar, işlem, alma, bazı desenleri paylaşabilir ve verileri depolamak.  İlk olarak, bu sistemler WINS'e aygıt algılayıcılar çeşitli yerel ayarların verilerini alma gerekir. Ardından, bu sistemler işlemek ve gerçek zamanlı Öngörüler çıkarmaya akış verilerini analiz edin. Verileri toplu analiz için soğuk depolama için sonra arşivlenir. Microsoft Azure IOT için uygulanabilir zengin Hizmetleri Azure Cosmos DB, Azure Event Hubs, Azure akış analizi, Azure bildirim hub'ı, Azure Machine Learning, Azure Hdınsight ve Power BI dahil olmak üzere kullanım sunar. 

![Azure Cosmos DB IOT başvuru mimarisi](./media/use-cases/iot.png)

Düşük gecikme süresine sahip yüksek verimlilik veri alımı sunduğu gibi veri WINS'e Azure Event Hubs tarafından alınan. Gerçek zamanlı bilgiler için işlenmesine gerek alınan verileri Azure akış analizi için gerçek zamanlı analiz funneled. Adhoc sorgulamak için Azure Cosmos Veritabanına veri yüklenebilir. Verileri Azure Cosmos Veritabanına yüklendikten sonra verileri Sorgulanacak hazırdır. Ayrıca, yeni veri ve varolan verilerde yapılan değişiklikleri üzerinde değişiklik Akış okunabilir. Değişiklik akışı bir kalıcı, sıralı bir düzende Cosmos DB koleksiyonlarda yapılan değişiklikler depolayan günlük ekleyin. Tüm veri veya yalnızca Azure Cosmos veritabanı veri değişiklikleri, başvuru verileri gerçek zamanlı analiz bir parçası olarak olarak kullanılabilir. Ayrıca, veriler daha fazla iyileştirilmiştir ve Azure Cosmos DB verileri Hdınsight'a Pig, Hive veya harita/azaltın işleri bağlanarak işlenir.  Gelişmiş Veri Azure Cosmos Veritabanına geri yüklenir ve raporlama için.   

Azure Cosmos DB, EventHubs ve Storm kullanan örnek bir IOT çözüm için bkz: [hdınsight storm örnekler deposu github'da](https://github.com/hdinsight/hdinsight-storm-examples/).

IOT için Azure teklifleri hakkında daha fazla bilgi için bkz: [, bilgisayarınızı interneti oluşturma](http://www.microsoft.com/server-cloud/internet-of-things.aspx). 

## <a name="retail-and-marketing"></a>Perakende ve pazarlama
Azure Cosmos DB Windows mağazası ve XBox Live çalıştıran Microsoft'un kendi e-ticaret platformlarda, yaygın olarak kullanılır. Bu ayrıca perakende sektörün ve ardışık düzen işleme sırayla kaynak Hizmeti'nden olay Kataloğu verilerini depolamak için kullanılır.

Katalog verileri kullanım senaryoları depolamak ve kişiler, yerler ve ürünler gibi varlıklar için bir dizi sorgulama içerir. Bazı katalog verilerini kullanıcı hesapları, ürün katalogları, IOT cihaz kayıt defterleri ve fatura malzemeleri sistemlerinin gösterilebilir. Bu veriler için öznitelikler değişebilir ve uygulama gereksinimlere uygun zamanla değişebilir.

Bir ürün kataloğu örneği öğesini bölümleri tedarikçi için göz önünde bulundurun. Her bölümü tüm bölümleri paylaşmak ortak özniteliklerin yanı sıra kendi özniteliklerine sahip olabilir. Ayrıca, belirli bir bölümü için öznitelikler için yeni bir model serbest bırakıldığında aşağıdaki yıl değiştirebilirsiniz. Esnek şemaları ve hiyerarşik verileri Azure Cosmos DB destekler ve bu nedenle, ürün kataloğu verilerini depolamak için uygundur.

![Azure Cosmos DB perakende katalog başvuru mimarisi](./media/use-cases/product-catalog.png)

Azure Cosmos DB sık kullanılan olay kullanarak güç olay yönelimli mimarileri için kaynak için kendi [akış değiştirmek](change-feed.md) işlevselliği. Değişiklik akış aşağı akış mikro güvenilir ve artımlı olarak ekler ve bir Azure Cosmos DB yapılan güncelleştirmeler (örneğin, sipariş olayları) okuma özelliği sağlar. Bu işlevsellik, kalıcı olay deposu bir ileti Aracısı durumu değiştirme olayları ve birçok mikro arasında sürücü sipariş işleme iş akışı sağlamak için de kullanılabilir (hangi uygulanabilir olarak [sunucusuz Azure işlevleri](http://azure.com/serverless)).

![Ardışık Düzen referans mimarisi sıralama Azure Cosmos DB](./media/use-cases/event-sourcing.png)

Ayrıca, Apache Spark işleri aracılığıyla büyük veri analizi için Hdınsight ile Azure Cosmos DB içinde depolanan verileri tümleştirilebilir. Azure Cosmos DB için Spark Bağlayıcısı hakkında daha fazla bilgi için bkz: [Cosmos DB ve Hdınsight Spark işini Çalıştır](spark-connector.md).

## <a name="gaming"></a>Oyun
Veritabanı katmanı oyun uygulamaları önemli bir bileşenidir. Modern oyunlar mobil/konsol istemcilerde grafik işleme gerçekleştirir, ancak oyun istatistikleri, sosyal medya tümleştirme ve yüksek puan liderlik gibi özelleştirilmiş ve kişiselleştirilmiş içerik sunmak üzere buluttaki kullanır. Oyunlar genellikle okumalar tek milisaniyelik gecikme gerektiren ve oyun-deneyimi bir çekici sağlamak için yazar. Oyun veritabanı hızlı ve yoğun istek hızları, yeni oyun başlatır sırasında artışlarını ve güncelleştirmeleri özellik kurabilmesi gerekir.

Azure Cosmos DB gibi oyunlar tarafından kullanılan [taramasını ölü: Hayır ADAM'ın kara](https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) tarafından [sonraki oyunlar](http://www.nextgames.com/), ve [hale 5: veli](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Azure Cosmos DB oyun geliştiricileri için aşağıdaki faydaları sağlar:

* Azure Cosmos DB ölçeklendirilmesi performans sağlayan yukarı veya aşağı özellikler esnek. Bu, tek bir API çağrısı yaparak güncelleştirme profili ve düzinelerce gelen istatistikleri eş zamanlı oyun severler milyonlarca işlemek oyunlar sağlar.
* Azure Cosmos DB milisaniyelik okuma destekler ve oyun sırasında tüm gecikmelere önlemeye yardımcı olmak yazar.
* Azure Cosmos DB'ın otomatik dizin oluşturma sağlar gerçek zamanlı birden çok farklı özelliklere göre filtre uygulamak için örneğin, kendi iç player kimlikleri tarafından oynatıcıları bulun veya kendi GameCenter, Facebook, Google kimlikleri veya sorgu tabanlı bir kılavuzu player üyeliği. Karmaşık dizin oluşturma veya parçalama altyapısı oluşturmadan bu mümkündür.
* Oyun Sohbet iletilerini, player Kılavuzu üyelikleri, tamamlandı zorluklar, yüksek puan liderlik ve sosyal grafikleri dahil olmak üzere sosyal özelliklerini esnek şemasıyla uygulamak daha kolay.
* Bir yönetilen platform olarak-hizmet (PaaS) olarak Azure Cosmos DB en az Kurulum ve Yönetim Hızlı yineleme için izin vermek için çalışmaz ve pazara süresini azaltmak gereklidir.

![Azure Cosmos DB oyun başvuru mimarisi](./media/use-cases/gaming.png)

## <a name="web-and-mobile-applications"></a>Web uygulamaları ve mobil uygulamalar
Azure Cosmos DB web ve mobil uygulamaları içinde yaygın olarak kullanılır ve üçüncü taraf hizmetleri ile zengin kişiselleştirilmiş deneyimleri geliştirmek için tümleştirme sosyal etkileşimleri modelleme için uygundur. Cosmos DB SDK'ları kullanılan yapı zengin iOS ve Android uygulamaları popüler kullanarak olabilir [Xamarin framework](mobile-apps-with-xamarin.md).  

### <a name="social-applications"></a>Sosyal uygulamaları
Bir ortak kullanım için Azure Cosmos DB depolamak ve oluşturulan kullanıcı içeriği (UGC) sorgulamak için web, mobil ve sosyal medya uygulamaları geçerlidir. Bazı UGC sohbet oturumları, tweet'leri, blog gönderileri, derecelendirme ve yorum gösterilebilir. Genellikle, sosyal medya uygulamalarda UGC serbest biçimli metin, özellikler, etiketler ve katı yapısı tarafından sınırlanmış değil ilişkileri karışım olur. İçerik sohbetleri gibi açıklamalar ve gönderileri Cosmos DB'de gerektirmeden dönüşümleri veya karmaşık nesne ilişkisel eşleme katmanlara depolanabilir.  Veri özellikleri eklenemez veya böylece hızlı geliştirme yükseltme geliştiricilerin uygulama kodu yineleme olarak gereksinimlerini karşılamak için kolayca değiştirilemez.  

Üçüncü taraf sosyal ağlarla tümleştirme uygulamalar bu ağlardan şemaları değiştirmeye yanıtlaması gerekir. Verileri otomatik olarak varsayılan olarak Cosmos DB sıralı olarak veri herhangi bir zamanda Sorgulanacak hazırdır. Bu nedenle, bu uygulamaları kendi gereksinimlerine göre projeksiyonlar almak için esnekliğe sahip olursunuz.

Birçok sosyal uygulamaları genel ölçeğinde çalıştırılacak ve tahmin edilemeyen kullanım biçimlerine sergiler. Veri deposu ölçeklendirme esneklik, uygulama katmanı kullanım talep eşleşecek şekilde olarak gereklidir.  Out ek veri bölümlerini Cosmos DB hesabı altında ekleyerek ölçeklendirebilirsiniz.  Ayrıca, birçok bölgeye ek Cosmos DB hesapları da oluşturabilirsiniz. Cosmos DB hizmet bölge kullanılabilirliği için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/#services).

![Azure Cosmos DB web uygulaması başvuru mimarisi](./media/use-cases/apps-with-global-reach.png)

### <a name="personalization"></a>Kişiselleştirme
Günümüzde, modern uygulamalar karmaşık görünümleri ve deneyimleri ile gelir. Bu, kullanıcı tercihlerine veya kapımıza yemek ve gereksinimlerini markalama genellikle dinamik. Bu nedenle, uygulama kullanıcı Arabirimi öğeleri ve deneyimleri etkili bir şekilde hızlı bir şekilde işlemek için kişiselleştirilmiş ayarları almak gerekir. 

JSON, Cosmos DB tarafından desteklenen bir biçim yalnızca basit değildir ancak JavaScript tarafından kolayca yorumlanabilen UI düzen verilerini göstermek için etkili bir biçimidir. Cosmos DB düşük gecikme süreli yazma işlemleri ile hızlı okuma izin ince ayarlanabilir tutarlılık düzeyleri sunar. Bu nedenle, kullanıcı Arabirimi Düzen veri Cosmos DB'de JSON belgeleri olarak kişiselleştirilmiş ayarları dahil olmak üzere depolama kablo bu verileri almak için etkili bir anlamına gelir.

![Azure Cosmos DB web uygulaması başvuru mimarisi](./media/use-cases/personalization.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB ile çalışmaya başlamak için izleyin bizim [hızlı başlangıçlar](create-sql-api-dotnet.md), hangi yol, bir hesap ve Cosmos DB ile çalışmaya başlama oluşturmada size. 

Veya daha fazla bilgi istiyorsanız aşağıdaki müşteri hikayeleri Cosmos DB kullanan müşteriler hakkında kullanılabilir:

* [Jet.com](https://jet.com). E-ticaret karşıt üst nokta eyes, Microsoft Bulutu üzerinde çalışan, genel bir ölçekte Cosmos DB yararlanır.
* [Asos.com](http://www.asos.com/). Asos.com bir İngiliz çevrimiçi moda ve güzelliği deposudur. Öncelikle Küçük yaştaki yetişkinler amaçlayan, Asos üzerinde 850 markalar ve bunun yanı sıra kendi dizi giysisinin ve Donatılar satıyor.
* [Toyota](https://www.toyota.com/). Toyota Motor Japonca öğesini üretici bir şirkettir. Toyota Cosmos DB genel IOT uygulama için kullanılabilir.
* [Citrix](https://customers.microsoft.com/story/citrix). Azure Service Fabric ve Azure Cosmos DB kullanarak çoklu oturum açma çözüm Citrix geliştirir
* [TEXA](https://customers.microsoft.com/story/texaspa) zamanı, para, gaz TEXA'ın devrim niteliğinde IOT çözüm araç sahipleri için yardımcı olur — ve büyük olasılıkla yaşar.
* [Domino'nın Pizza](https://www.dominos.com). Domino'nın Pizza Inc. Amerikan pizza Restoran zinciri ' dir.
* [Johnson denetimleri](http://www.johnsoncontrols.com). Johnson denetimler, genel büyük teknoloji ve çok çeşitli müşteriler 150'den fazla ülkede hizmet veren çok endüstri lideri ' dir.
* [Microsoft Windows, Evrensel deposu, Azure IOT Hub, Xbox Live ve diğer Internet ölçekli Hizmetler](https://azure.microsoft.com/blog/how-azure-documentdb-planet-scale-nosql-helps-run-microsoft-s-own-businesses/). Nasıl Microsoft Azure Cosmos DB kullanarak yüksek düzeyde ölçeklenebilir hizmetler oluşturur.
* [Microsoft Data ve analiz takım](https://customers.microsoft.com/story/microsoftdataandanalytics). Microsoft'un verileri ve çözümlemeler takım başarır Azure Cosmos DB ile planet ölçekli büyük veri toplama
* [Sulekha.com](https://customers.microsoft.com/story/sulekha-uses-azure-documentdb-to-connect-customers-and-businesses-across-india). Sulekha Azure Cosmos DB müşteriler ve işletmeler Hindistan bağlanmak için kullanır.
* [NewOrbit](https://customers.microsoft.com/story/neworbit-takes-flight-with-azure-documentdb). NewOrbit Azure Cosmos DB ile uçuş alır.
* [Affinio](https://customers.microsoft.com/doclink/affinio-switches-from-aws-to-azure-documentdb-to-harness-social-data-at-scale). Affinio AWS ölçekte sosyal veriler harekete geçirmek Azure Cosmos DB'de geçer.
* [Sonraki oyunları](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). Walking atılacak: Hayır adam Halk kara oyun yükselmesi # 1 Azure Cosmos DB tarafından desteklenir.
* [Hale](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Nasıl hale 5 Azure Cosmos DB kullanarak sosyal oyunlar uygulanmadı.
* [Cortana Analytics Galerisi](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana Analytics Galerisi - Azure Cosmos DB'de yerleşik ölçeklenebilir topluluk sitesi.
* [Kolay](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Tümleştirici önde gelen esnek bulut teknolojileri ile dakika cinsinden çokuluslu firmalarından genel bilgiler sağlar.
* [Haber Cumhuriyeti](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Katılımcı kullanıcılarının bu amaçla bilgi sağlamak için haber Intelligence ekleme. 
* [GG uluslararası](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Dünya çapında tutarlı renk için önemli markalar gg için etkinleştirin. Ve Azure'a gg kapatır.
* [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Genel öncü Telenor bulut başlangıç hızıyla taşımak için kullanır. 
* [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). Mağaza gelecekteki hızlı arama ve kolay veri akışını üzerinde çalışır.
* [Nucleo](https://customers.microsoft.com/story/azure-based-software-platform-breaks-down-barriers-bet). İşletmelerin ve müşteriler arasındaki engelleri aşağı Azure tabanlı yazılım platformu keser.
* [Weka](https://customers.microsoft.com/story/weka-smart-fridge-improves-vaccine-management-so-more-people-can-be-protected-against-diseases). Daha fazla kişinin diseases karşı korunmak için weka akıllı Fridge vaccine yönetim artırır.
* [Turuncu Tribes](https://customers.microsoft.com/story/theres-more-to-that-food-app-than-meets-the-eye-or-the-mouth). Bu yemek uygulama göz veya ağzınıza karşılayan daha çok daha fazlası vardır.
* [Gerçek Madrid](https://customers.microsoft.com/story/real-madrid-brings-the-stadium-closer-to-450-million-f). Gerçek Madrid stadyum yakın dünya Microsoft Cloud ile 450 milyon fanlar beraberinde getirir.
* [Tuku](https://customers.microsoft.com/story/tuku-makes-car-buying-fun-with-help-from-azure-services). Azure hizmetlerinden Yardım keyif satın araba TUKU yapar
