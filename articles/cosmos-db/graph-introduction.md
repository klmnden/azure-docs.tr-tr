---
title: "Azure Cosmos DB Graph API giriş | Microsoft Docs"
description: "Sorgu, depolamak ve yoğun grafikleri Apache TinkerPop Gremlin grafik sorgu dilini kullanarak, düşük gecikme süresine sahip çapraz geçiş için Azure Cosmos DB nasıl kullanabileceğinizi öğrenin."
services: cosmos-db
author: dennyglee
documentationcenter: 
ms.assetid: b916644c-4f28-4964-95fe-681faa6d6e08
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/15/2017
ms.author: denlee
ms.openlocfilehash: 71d9d03b45d8c4fcf8acb41871dcf3f1304955aa
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="introduction-to-azure-cosmos-db-graph-api"></a>Azure Cosmos DB giriş: grafik API'si

[Azure Cosmos DB](introduction.md) görev açısından kritik uygulamalar için Microsoft Genel dağıtılmış, multimodel veritabanı hizmeti. Azure Cosmos DB desteğiyle tüm aşağıdaki özellikleri sağlar [endüstri lideri SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/):

* [Anahtar teslimi genel dağıtım](distribute-data-globally.md)
* [Üretilen iş ve depolama esnek ölçeklendirme](partition-data.md) dünya çapında
* Tek basamaklı milisaniyelik gecikme 99 adresindeki
* [Beş iyi tanımlanmış tutarlılık düzeyleri](consistency-levels.md)
* Yüksek oranda kullanılabilirlik garanti 

Azure Cosmos DB, şema ve dizin yönetimiyle ilgilenmenize gerek kalmadan [otomatik olarak verilerin dizinini oluşturur](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Multimodel ve belge, anahtar-değer, grafik ve sütunlu veri modelleri destekler.

Burada Kirill Gavrylyuk Azure Cosmos DB grafiklerde kullanmaya başlama açıklanmaktadır aşağıdaki videoyu izleyin öneririz:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Graphs-with-Azure-Cosmos-DB-Gremlin-API/player]
> 
> 

Azure Cosmos DB grafik API'si sağlar:

- Modelleme grafik.
- Çapraz geçiş API'leri.
- Anahtar teslimi genel dağıtım.
- Esnek depolama ve işleme gecikmeleri okuma 10 MS'den kısa ve 99 en az 15 ms ölçeklendirme.
- Otomatik anlık sorgu kullanılabilirlik ile dizin oluşturma.
- İnce ayarlanabilir tutarlılık düzeyleri.
- Kapsamlı SLA gevşek tutarlılık ve %99.999 % 99,99 kullanılabilirlik SLA tüm tek bölge ve tüm bölgeli hesapları için de dahil olmak üzere tüm bölgeli veritabanı hesaplarda kullanılabilirlik okuyun.

Azure Cosmos DB sorgulamak için kullanabileceğiniz [Apache TinkerPop](http://tinkerpop.apache.org) grafik geçişi dil [Gremlin](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps), veya diğer TinkerPop uyumlu grafik sistemleri [Apache Spark GraphX](spark-connector-graph.md).

Bu makalede Azure Cosmos DB grafik API'sini genel bir bakış sağlar ve köşeleri ve kenarları milyarlarca yoğun grafiklerle depolamak için nasıl kullanabileceğinizi açıklar. Grafikleri milisaniyelik gecikme süresi ile sorgular ve grafik yapısı ve şema kolayca geliştirin.

## <a name="graph-database"></a>Grafik veritabanı
Gerçek dünyada göründüğü gibi veri doğal olarak bağlanır. Geleneksel veri modelleme varlıklarını odaklanır. Birçok uygulama için aynı zamanda model veya doğal olarak varlıkları ve ilişkileri modellemek için bir gereksinimi yoktur.

A [grafik](http://mathworld.wolfram.com/Graph.html) oluşur yapısıdır [köşeleri](http://mathworld.wolfram.com/GraphVertex.html) ve [kenarları](http://mathworld.wolfram.com/GraphEdge.html). Köşeleri ve kenarları özelliklerinin rastgele bir sayı olabilir. Köşeleri bir kişi, yer veya olay gibi ayrı nesneleri gösterir. Kenarları köşeleri arasındaki ilişkileri gösterir. Örneğin, bir kişinin başka bir kişiye bildirin, bir olay söz konusu ve kısa süre önce bir konumda bırakıldı. Özellikler köşeleri ve kenarları hakkında bilgi express. Örnek özellikleri adı, yaşı ve zaman damgası ve/veya bir ağırlık sahip kenar köşe içerir. Bu model olarak daha resmi olarak bilinen bir [özelliği grafik](http://tinkerpop.apache.org/docs/current/reference/#intro). Azure Cosmos DB özelliği grafik modelini destekler.

Örneğin, aşağıdaki örnek grafiği kişiler, mobil aygıtlar, ilgi alanları ve işletim sistemleri arasındaki ilişkiler gösterilmektedir:

![Kişilerin, cihazların ve ilgi gösteren örnek veritabanı](./media/graph-introduction/sample-graph.png)

Grafikler, çok çeşitli veri kümelerinde Bilimsel, teknoloji ve iş anlamak kullanışlıdır. Grafik veritabanları, bunları birçok senaryoları için kullanışlıdır grafikleri doğal ve verimli bir şekilde depolamak ve modeli sağlar. Bu kullanım gereksinimini şema esnekliği ve hızlı yineleme genellikle de örnekleri çünkü grafik veritabanları genellikle NoSQL veritabanlarıdır.

Grafikleri Romanım ve teknik modelleme güçlü verileri sunar. Ancak bu olgu kendi başına bir grafik veritabanını kullanmak için yeterli bir nedenle değil. Birçok kullanım örnekleri ve grafik çapraz geçişlerine içeren desenler için grafikleri geleneksel NoSQL ve SQL veritabanları büyüklük daha iyi performans gösterir. Bu performans farkı daha fazla arkadaş, arkadaş gibi birden fazla ilişkiye çapraz geçiş yapan zaman yükseltilmiş.

Grafik algoritmalarıyla grafik veritabanları hızlı çapraz geçişlerine birleştirebilirsiniz derinliği ilk arama, avantajlarına ilk arama ve sosyal ağ, içerik yönetimi, Jeo-uzamsal, gibi çeşitli etki alanlarına sorunlarını çözmek için Dijkstra'nın algoritması gibi ve öneriler sunar.

## <a name="planet-scale-graphs-with-azure-cosmos-db"></a>Azure Cosmos DB ile Planet ölçek grafikleri
Azure Cosmos DB genel dağıtım, depolama ve işleme, otomatik dizin oluşturma ve sorgu, ince ayarlanabilir tutarlılık düzeyleri ve TinkerPop standardı desteği ölçeklendirme esnek sunan tam olarak yönetilen grafik bir veritabanıdır.

![Azure Cosmos DB graph mimarisi](./media/graph-introduction/cosmosdb-graph-architecture.png)

Azure Cosmos DB pazardaki diğer grafik veritabanlarına karşılaştırıldığında aşağıdaki farklı özellikleri sunar:

* Esnek bir şekilde ölçeklenebilir işleme ve depolama

 Tek bir sunucu kapasitesinin ötesine ölçeklendirme gerçek dünya grafiklerde gerekir. Azure Cosmos DB ile grafikleri sorunsuz bir şekilde birden çok sunucu arasında ölçeklendirebilirsiniz. Bağımsız olarak, erişim düzenlerini esas alarak, grafik üretimini de ölçeklendirebilirsiniz. Azure Cosmos DB neredeyse sınırsız depolama boyutlarına ve sağlanan işlemeye ölçeklenebilir grafik veritabanlarını destekler.

* Bölgeli çoğaltma

 Azure Cosmos DB grafik verilerinizi hesabınızla ilişkilendirdiğiniz tüm bölgelere saydam olarak çoğaltır. Çoğaltma, verilere genel erişim gerektiren uygulamalar geliştirmenize olanak sağlar. Tutarlılık, kullanılabilirlik ve performans ve karşılık gelen garanti alanlarında bileşim vardır. Azure Cosmos DB çok girişli API'leri ile saydam bölgesel yük devretme sağlar. Dünya çapında işleme ve depolama özellikler esnek ölçeklendirebilirsiniz.

* Hızlı sorguları ve çapraz geçişlerine tanıdık Gremlin sözdizimi ile

 Heterojen köşeleri ve kenarları depolayın ve tanıdık bir Gremlin söz dizimi aracılığıyla bu belgeleri sorgulayın. Azure Cosmos DB otomatik olarak tüm içerik için dizin oluşturmak için bir derecede eşzamanlı, kilidi serbest, günlük yapılı dizin oluşturma teknolojisini kullanır. Bu özellik, gerçek zamanlı zengin sorgulara ve çapraz geçişlerine şema ipuçları, ikincil dizinler veya görünümler belirtmek zorunda kalmadan sağlar. Daha fazla bilgi edinin [sorgu grafikleri Gremlin kullanarak](gremlin-support.md).

* Tam olarak yönetilen

 Azure Cosmos DB veritabanı ve makine kaynaklarını yönetme ihtiyacını ortadan kaldırır. Tam olarak yönetilen bir Microsoft Azure hizmet olarak gereken sanal makineleri yönetmek, dağıtma ve yazılım yapılandırmanız, ölçeklendirmeyi yönetmeniz veya karmaşık veri katmanı yükseltmeleriyle uğraşmanız yok. Her grafik otomatik olarak yedeklenmesini ve bölgesel arızalara karşı korunur. Kolayca Azure Cosmos DB hesap eklemek ve işletim ve veritabanınızı yönetmek yerine, uygulamanızın üzerinde odaklanabiliriz gerek duyduğunuz kapasite sağlayın.

* Otomatik dizin oluşturma

 Varsayılan olarak, Azure Cosmos DB otomatik olarak tüm özellikleri düğümler ve grafik kenarları içinde dizinler ve değil beklediğiniz veya herhangi bir şemayı ya da ikincil dizinlerin oluşturulmasını gerektirir.

* Apache TinkerPop ile uyumluluk

 Azure Cosmos DB yerel olarak açık kaynak Apache TinkerPop standart destekler ve diğer TinkerPop etkin grafik sistemleriyle tümleştirilebilir. Bu nedenle, kolayca Titan veya Neo4j, gibi başka bir grafik veritabanından geçirmek veya gibi grafik analytics çerçeveleri ile Azure Cosmos DB kullanılsın [Apache Spark GraphX](spark-connector-graph.md).

* İnce ayarlanabilir tutarlılık düzeyleri

 Tutarlılık ve performans arasında en iyi kolaylığını elde etmek için beş iyi tanımlanmış tutarlılık düzeylerini seçin. Azure Cosmos DB sorgular ve okuma işlemleri için beş farklı tutarlılık düzeyi sunar: güçlü, sınırlanmış eskime durumu, oturum, tutarlı ön ek ve son. Bu ayrıntılı ve iyi tanımlanmış tutarlılık düzeyleri tutarlılık, kullanılabilirlik ve gecikme süresi arasında ses bileşim yapmanıza izin vermiyor. Daha fazla bilgi için bkz. [DocumentDB'de kullanılabilirlik ve performansı en üst düzeye çıkarmak için tutarlılık düzeylerini kullanma](consistency-levels.md).

Azure Cosmos DB belge ve grafik, aynı kapsayıcıları/veritabanları içinde gibi birden çok modelleri de kullanabilirsiniz. Belgeleri yan yana grafik verilerini depolamak için bir belge koleksiyonu kullanabilirsiniz. Bir grafik aynı verileri sorgulamak için JSON SQL sorguları ve Gremlin sorguları kullanabilirsiniz.

## <a name="get-started"></a>başlarken
Azure komut satırı arabirimi (CLI), Azure PowerShell veya Azure portalında desteğiyle grafik API'si için Azure Cosmos DB hesapları oluşturmak için kullanabilirsiniz. Hesapları oluşturduktan sonra Azure portalında bir hizmet uç noktası gibi sağlar `https://<youraccount>.graphs.azure.com`, WebSocket ön uç için Gremlin sağlar. TinkerPop uyumlu araçlarınızı gibi yapılandırabilirsiniz [Gremlin konsol](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console), bu uç noktasına bağlanmak ve Java, Node.js veya herhangi bir Gremlin istemci sürücüsü uygulamaları oluşturmak için.

Aşağıdaki tabloda Azure Cosmos DB karşı kullanabilirsiniz popüler Gremlin sürücüleri gösterir:

| İndir | Belgeler |
| --- | --- |
| [Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) |[Gremlin JavaDoc](http://tinkerpop.apache.org/javadocs/current/full/) |
| [Node.js](https://www.npmjs.com/package/gremlin) |[Github'da gremlin JavaScript](https://github.com/jbmusso/gremlin-javascript) |
| [Gremlin konsol](https://tinkerpop.apache.org/downloads.html) |[TinkerPop belgeleri](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console) |

Azure Cosmos DB ayrıca Gremlin genişletme yöntemleri üstüne sahip bir .NET kitaplığı sağlar [Azure Cosmos DB SDK'ları](documentdb-sdk-dotnet.md) NuGet aracılığıyla. Bu kitaplığı, "DocumentDB verileri bölümlere doğrudan bağlanmak için kullanabileceğiniz bir işlem" Gremlin sunucusu sağlar.

| İndir | Belgeler |
| --- | --- |
| [.NET](https://www.nuget.org/packages/Microsoft.Azure.Graphs/) |[Microsoft.Azure.Graphs](https://msdn.microsoft.com/library/azure/dn948556.aspx) |

Kullanarak [Azure Cosmos DB öykünücüsü](local-emulator.md), geliştirmek ve bir Azure aboneliği oluşturmak veya herhangi bir maliyet olmadan yerel olarak test etmek için yukarıdaki .NET grafik API'sini kullanabilirsiniz. Uygulamanızı Öykünücüde nasıl çalıştığını ile memnun kaldığınızda, bulutta bir Azure Cosmos DB hesabı kullanmaya geçiş yapabilirsiniz.

> [!NOTE]
> Gremlin sorguları doğrulama desteği [Azure Cosmos DB öykünücüsü](local-emulator.md) yalnızca .NET grafik API'si kullanılabilir.

## <a name="scenarios-for-graph-support-of-azure-cosmos-db"></a>Azure Cosmos DB grafik desteği için senaryolar
Azure Cosmos DB grafik desteği kullanıldığı bazı senaryolar verilmiştir:

* Sosyal ağlar

 Müşterileriniz ve diğer kişilerle kendi etkileşimler hakkındaki verileri birleştirerek, kişiselleştirilmiş deneyimleri geliştirmek, müşteri davranışı tahmin etmek veya kişiler başkalarıyla benzer ilgi alanları ile bağlanabilirsiniz. Azure Cosmos DB sosyal ağlar yönetmek ve müşteri tercihlerini ve verileri izlemek için kullanılabilir.

* Öneri altyapısı

 Bu senaryo perakende sektördeki yaygın olarak kullanılır. Ürünleri, kullanıcıları ve satın alma, gözatma veya bir öğe derecelendirme gibi kullanıcı etkileşimleri hakkında bilgi birleştirerek özelleştirilmiş öneriler oluşturabilirsiniz. Düşük gecikme süresi, esnek ölçek ve yerel Azure Cosmos DB grafik desteği bu etkileşimleri modelleme için idealdir.

* Jeo-uzamsal

 İçindeki bir alanı ilgi konumunu bulamıyor veya iki konum arasında kısa ve en uygun rotayı bulmak birçok uygulama telekomünikasyon, lojistik ve seyahat planlama gerekir. Azure Cosmos DB bu sorunları için doğal bir Sığdırma ' dir.

* Nesnelerin İnterneti

 Ağ ve bir grafik Modellenen IOT cihazları arasındaki bağlantıları ile daha iyi anlamasına yardımcı aygıtları ve varlıkları durumunu oluşturabilirsiniz. Ayrıca nasıl ağın bir parçası olarak değişikliklerin başka bir bölümü olası etkileyebileceğini öğrenin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB grafik desteği hakkında daha fazla bilgi için bkz:

* Kullanmaya başlama [Azure Cosmos DB grafik Öğreticisi](create-graph-dotnet.md).
* Nasıl yapılır hakkında bilgi edinin [Gremlin kullanarak Azure Cosmos DB grafiklerde sorgu](gremlin-support.md).
