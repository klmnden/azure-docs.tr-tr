---
title: Azure Cosmos DB Graph API’lere Giriş | Microsoft Belgeleri
description: Düşük gecikme süresine sahip yoğun grafikleri Apache TinkerPop’un Gremlin grafik sorgu dilini kullanarak depolamak, sorgulamak ve dolaştırmak için Azure Cosmos DB’yi nasıl kullanabileceğinizi öğrenin.
services: cosmos-db
author: LuisBosquez
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.devlang: na
ms.topic: overview
ms.date: 01/05/2017
ms.author: lbosq
ms.openlocfilehash: 333bb4074ac741e854ff56c7c397b0e3be247f1b
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857159"
---
# <a name="introduction-to-azure-cosmos-db-graph-api"></a>Azure Cosmos DB’ye Giriş: Graph API

[Azure Cosmos DB](introduction.md), Microsoft'un görev açısından kritik uygulamalar için sunduğu, genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB, [sektörün en iyi SLA’sıyla](https://azure.microsoft.com/support/legal/sla/cosmos-db/) desteklenen şu özellikleri sunar:

* [Anahtar teslimi genel dağıtım](distribute-data-globally.md)
* Tüm dünyada [aktarım hızını ve depolamayı esnek bir şekilde ölçeklendirme](partition-data.md)
* 99. yüzdebirlikte tek basamaklı milisaniyelik gecikme süresi
* [Beş iyi tanımlanmış tutarlılık düzeyi](consistency-levels.md)
* Garantili yüksek kullanılabilirlik 

Azure Cosmos DB, şema ve dizin yönetimiyle ilgilenmenize gerek kalmadan [otomatik olarak verilerin dizinini oluşturur](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Çok modelli olan bu hizmet belge, anahtar-değer, grafik ve sütunlu veri modellerini destekler.

Kirill Gavrylyuk'un Azure Cosmos DB’de grafiklerle çalışmaya başlama konulu aşağıdaki videosunu izlemenizi öneririz:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Graphs-with-Azure-Cosmos-DB-Gremlin-API/player]
> 
> 

Azure Cosmos DB Graph API şunları sağlar:

- Grafik modelleme.
- Dolaşma API’si.
- Anahtar teslimi genel dağıtım.
- Okumada 10 milisaniyeden kısa gecikme süresi ve 99. yüzdebirlikte 15 milisaniyeden kısa gecikme süresiyle depolamayı ve aktarım hızını esnek olarak ölçeklendirme.
- Sorguların anlık olarak kullanılabilmesi sayesinde otomatik dizinleme.
- Ayarlanabilir tutarlılık düzeyleri.
- Rahat bir tutarlılıkla tek tek tüm bölge hesapları ve çok bölgeli tüm hesaplar için %99,99 kullanılabilirlik SLA'sı ve çok bölgeli tüm veritabanı hesaplarında %99,999 okunabilirlik olanaklarını içeren kapsamlı SLA’lar.

Azure Cosmos DB'yi sorgulamak için [Apache TinkerPop](http://tinkerpop.apache.org) grafik içinde dolaşma dilini veya [Gremlin](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps)'i kullanabilirsiniz.

Bu makale, Azure Cosmos DB Graph API'ye genel bir bakış sağlar ve milyarlarca köşesi ve kenarı olan yoğun grafikleri depolamak için bunu nasıl kullanabileceğinizi açıklar. Grafikleri milisaniyelik gecikme süresi ile sorgulayabilir, grafik yapısını ve şemasını kolayca geliştirebilirsiniz.

## <a name="graph-database"></a>Grafik veritabanı
Gerçek dünyada görünen veriler doğal olarak bağlıdır. Geleneksel veri modelleme, varlıklara odaklanır. Birçok uygulamada aynı zamanda modelleme veya hem varlıkları hem de ilişkileri doğal olarak modelleme gereksinimi söz konusudur.

[Grafikler](http://mathworld.wolfram.com/Graph.html), [köşelerden](http://mathworld.wolfram.com/GraphVertex.html) ve [kenarlardan](http://mathworld.wolfram.com/GraphEdge.html) oluşan yapılardır. Köşelerin ve kenarların rastgele bir sayıda özellikleri olabilir. Köşeler, bir kişi, yer veya etkinlik gibi belirli nesneleri gösterir. Kenarlar ise köşeler arasındaki ilişkileri gösterir. Bir kişinin başka bir kişiyi tanıması, bir etkinliğe katılması veya kısa süre önce bir konumda bulunması bağlantılar buna örnek gösterilebilir. Özellikler, köşeler ve kenarlar hakkında bilgi sağlar. Bir köşenin adı ve yaşı ya da bir kenarın zaman damgası ve/veya ağırlığı özelliklere örnek gösterilebilir. Bu modele [özellik grafiği](http://tinkerpop.apache.org/docs/current/reference/#intro) de denir. Azure Cosmos DB, özellik grafiği modelini destekler.

Aşağıdaki örnek grafik kişiler, mobil cihazlar, ilgi alanları ve işletim sistemleri arasındaki ilişkileri gösterir:

![Kişileri, cihazları ve ilgi alanlarını gösteren örnek grafik](./media/graph-introduction/sample-graph.png)

Grafikler bilim, teknoloji ve iş alanında çok çeşitli veri kümelerinin anlaşılmasına yardımcı olur. Grafikleri doğal ve verimli bir şekilde modellemenizi ve depolamanızı sağlayan grafik veritabanları, birçok senaryoda işe yarar. Bu kullanım örnekleri için çoğu zaman şema esnekliği ve hızlı yineleme gerektiğinden, grafik veritabanları genelde NoSQL veritabanlarıdır.

Grafikler, yepyeni ve güçlü bir veri modelleme tekniği sunar. Ancak yalnızca bu, grafik veritabanı kullanmak için yeterli bir sebep değildir. Grafik dolaşımı gerektiren birçok kullanım örneği ve düzende, grafiklerin performansı geleneksel SQL ve NoSQL veritabanlarına kıyasla çok daha yüksektir. Bu performans farkı, arkadaşın arkadaşı gibi birden fazla ilişki dolaştırıldığında daha da fark edilir boyuta gelir.

Sosyal ağ, içerik yönetimi, jeo-uzamsal ve öneriler gibi çeşitli etki alanlarındaki sorunları çözmek için grafik veritabanlarının sağladığı hızlı dolaşımları derinlik öncelikli arama, genişlik öncelikli arama ve Dijkstra’nın algoritması gibi grafik algoritmalarıyla birleştirebilirsiniz.

## <a name="planet-scale-graphs-with-azure-cosmos-db"></a>Azure Cosmos DB ile küresel ölçekte grafikler
Azure Cosmos DB; genel dağıtım, depolama ve aktarım hızında esnek ölçeklendirme, otomatik dizinleme ve sorgu, ayarlanabilir tutarlılık düzeyleri ve TinkerPop standardı desteği sunan, tam olarak yönetilen bir grafik veritabanıdır.

![Azure Cosmos DB grafik mimarisi](./media/graph-introduction/cosmosdb-graph-architecture.png)

Azure Cosmos DB pazardaki diğer grafik veritabanlarıyla karşılaştırıldığında aşağıdaki farklı özellikleri sunar:

* Aktarım hızı ve depolamayı esnek bir şekilde ölçeklendirme

 Gerçek dünyadaki grafiklerin, tek bir sunucunun kapasitesinin yetmeyeceği biçimde ölçeklendirilmesi gerekir. Azure Cosmos DB ile grafikleri sorunsuz bir şekilde birden çok sunucu arasında ölçeklendirebilirsiniz. Erişim düzenlerinize dayalı olarak grafiğinizin aktarım hızını da bağımsız biçimde ölçeklendirebilirsiniz. Azure Cosmos DB, neredeyse sınırsız depolama boyutlarına ve sağlanmış aktarım hızına ölçeklenebilen grafik veritabanlarını destekler.

* Çok bölgeli çoğaltma

 Azure Cosmos DB, grafik verilerinizi hesabınızla ilişkilendirdiğiniz tüm bölgelere saydam olarak çoğaltır. Çoğaltma, verilere genel erişim gerektiren uygulamalar geliştirmenize olanak sağlar. Tutarlılık, kullanılabilirlik, performans ve karşılık gelen garanti alanlarında bazı kayıplar olabilir. Azure Cosmos DB, birden çok girişli API’ler için saydam bölgesel yük devretme teklifleri sağlar. Aktarım hızı ve depolamayı esnek bir şekilde ölçeklendirebilirsiniz.

* Bildiğiniz Gremlin sözdizimi ile hızlı sorguları ve çapraz geçişlerine

 Heterojen köşeleri ve kenarları depolayın ve bildiğiniz bir Gremlin söz dizimiyle bu belgeleri sorgulayın. Azure Cosmos DB, tüm içeriğin otomatik olarak dizinini oluşturmak için yüksek derecede eşzamanlı, kilitsiz, günlük yapılı bir dizin oluşturma teknolojisi kullanır. Bu özellik şema ipuçları, ikincil dizinler veya görünümler belirtmek gerekmeden gerçek zamanlı zengin sorgulara ve geçişlere olanak sağlar. Daha fazla bilgi için bkz. [Gremlin kullanarak sorgu grafikleri](gremlin-support.md).

* Tam olarak yönetilir

 Azure Cosmos DB, veritabanı ve makine kaynaklarını yönetme ihtiyacını ortadan kaldırır. Bu tam olarak yönetilen bir Microsoft Azure hizmeti olduğundan sanal makineleri yönetmeniz, yazılımları dağıtıp yapılandırmanız, ölçeklendirmeyi yönetmeniz veya karmaşık veri katmanı yükseltmeleriyle uğraşmanız gerekmez. Tüm grafikler otomatik olarak yedeklenir ve bölgesel arızalara karşı korunur. İhtiyacınız oldukça kolaylıkla bir Azure Cosmos DB hesabı ve sağlama kapasitesi ekleyebilirsiniz, böylece veritabanınızı çalıştırmak ve yönetmek yerine uygulamanıza odaklanmanız sağlanır.

* Otomatik dizin oluşturma

 Azure Cosmos DB, varsayılan olarak grafiğin düğümlerindeki ve kenarlarındaki tüm özelliklerin otomatik olarak dizinini oluşturur ve herhangi bir şemayı ya da ikincil dizinlerin oluşturulmasını beklemez veya gerektirmez.

* Apache TinkerPop ile uyumluluk

 Azure Cosmos DB, yerel olarak Apache TinkerPop standart desteği sunar ve TinkerPop’un etkinleştirildiği diğer grafik sistemleriyle tümleştirilebilir. Bu sayede kolayca Titan veya Neo4j gibi başka bir grafik veritabanından geçiş yapabilir veya Azure Cosmos DB’yi Apache Spark GraphX gibi grafik analizi çerçeveleriyle kullanabilirsiniz.

* Ayarlanabilir tutarlılık düzeyleri

 Tutarlılık ve performans arasında en iyi dengeyi elde etmek için iyi tanımlanmış beş tutarlılık düzeyi arasından seçim yapın. Azure Cosmos DB sorgular ve okuma işlemleri için beş farklı tutarlılık düzeyi sunar: güçlü, sınırlanmış eskime durumu, oturum, tutarlı ön ek ve son. Bu ayrıntılı ve iyi tanımlanmış tutarlılık düzeyleri tutarlılık, kullanılabilirlik ve gecikme süresi arasında sağlam bir denge kurmanıza olanak sağlar. [Azure Cosmos DB'deki ayarlanabilir tutarlılık düzeyleri](consistency-levels.md) sayfasına giderek daha fazla bilgi edinin.

Azure Cosmos DB, aynı kapsayıcıların/veritabanlarının içinde belge ve grafik gibi birden çok modeli de kullanabilir. Grafik verilerini belgelerle yan yana depolamak için bir belge kapsayıcısı kullanabilirsiniz. Aynı verileri grafik olarak sorgulamak için JSON üzerinden SQL sorgularını ve Gremlin sorgularını kullanabilirsiniz.

## <a name="get-started"></a>başlarken
Azure Cosmos DB hesabı oluşturmak için Azure komut satırı arabirimini (CLI), Azure PowerShell’i veya grafik API’si desteği sunan Azure portalı kullanabilirsiniz. Hesapları oluşturduktan sonra Azure portal `https://<youraccount>.gremlin.cosmosdb.azure.com` gibi bir hizmet uç noktası sunar ve bu da Gremlin için bir WebSocket ön ucu sağlar. Uç noktasına bağlanmak ve Java, Node.js veya herhangi bir Gremlin istemci sürücüsünde uygulama oluşturmak için [Gremlin Konsolu](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console) gibi TinkerPop ile uyumlu araçlarınızı yapılandırabilirsiniz.

Aşağıdaki tabloda Azure Cosmos DB’ye karşı kullanabileceğiniz popüler Gremlin sürücüleri gösterilir:

| İndirme | Belgeler | Başlarken |
| --- | --- | --- |
| [.NET](http://tinkerpop.apache.org/docs/3.3.1/reference/#gremlin-DotNet) | [GitHub’da Gremlin.NET](https://github.com/apache/tinkerpop/tree/master/gremlin-dotnet) | [.NET kullanarak Grafik oluşturma](create-graph-dotnet.md) |
| [Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) | [Gremlin JavaDoc](http://tinkerpop.apache.org/javadocs/current/full/) | [Java kullanarak Grafik oluşturma](create-graph-java.md) |
| [Node.js](https://www.npmjs.com/package/gremlin) | [GitHub’da Gremlin-JavaScript](https://github.com/jbmusso/gremlin-javascript) | [Node.js kullanarak Grafik oluşturma](create-graph-nodejs.md) |
| [Python](http://tinkerpop.apache.org/docs/3.3.1/reference/#gremlin-python) | [Gremlin-Python on GitHub](https://github.com/apache/tinkerpop/tree/master/gremlin-python) | [Python kullanarak Grafik oluşturma](create-graph-python.md) |
| [PHP](https://packagist.org/packages/brightzone/gremlin-php) | [Github'da Gremlin-PHP](https://github.com/PommeVerte/gremlin-php) | [PHP kullanarak Grafik oluşturma](create-graph-php.md) |
| [Gremlin konsolu](https://tinkerpop.apache.org/downloads.html) | [TinkerPop belgeleri](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console) |  [Gremlin konsolunu kullanarak Grafik oluşturma](create-graph-gremlin-console.md) |

## <a name="scenarios-for-graph-support-of-azure-cosmos-db"></a>Azure Cosmos DB’nin grafik desteğine yönelik senaryolar
Azure Cosmos DB’nin grafik desteğinin kullanılabileceği bazı senaryolar aşağıda verilmiştir:

* Sosyal ağlar

 Müşterileriniz ve diğer kişilerle etkileşimleri hakkındaki verileri birleştirerek kişiselleştirilmiş deneyimleri geliştirebilir, müşteri davranışını tahmin edebilir veya kişileri benzer ilgi alanlarına sahip başkalarına bağlayabilirsiniz. Azure Cosmos DB, sosyal ağları yönetmek ve müşteri tercihlerini ve verilerini izlemek için kullanılabilir.

* Öneri altyapıları

 Bu senaryo, perakende sektöründe yaygın olarak kullanılır. Ürünler, kullanıcılar ve satın alma, gözatma veya bir öğeyi derecelendirme gibi kullanıcı etkileşimleri hakkındaki bilgileri birleştirerek özelleştirilmiş öneriler oluşturabilirsiniz. Azure Cosmos DB’nin düşük gecikme süresi, esnek ölçek ve yerel grafik desteği, bu etkileşimleri modelleme için idealdir.

* Jeo-uzamsal

 Telekomünikasyon, lojistik ve seyahat planlama alanlarındaki birçok uygulamanın, bir bölgede ilgi çekici bir konum bulması veya iki konum arasındaki en kısa/en uygun yolu belirlemesi gerekir. Azure Cosmos DB, bu sorunları için idealdir.

* Nesnelerin İnterneti

 IOT cihazları arasındaki ağ ve bağlantılar bir grafik olarak modellendiğinde, cihazlarınızın ve varlıklarınızın durumunu daha iyi kavrayabilirsiniz. Ayrıca ağın bir bölümündeki değişikliklerin başka bir bölümü nasıl etkileyebileceğini de öğrenebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB’de grafik desteği hakkında daha fazla bilgi için bkz.

* [Azure Cosmos DB grafik öğreticisi](create-graph-dotnet.md) ile çalışmaya başlayın.
* [Azure Cosmos DB’de Gremlin kullanarak grafikleri sorgulamayı](gremlin-support.md) öğrenin.
