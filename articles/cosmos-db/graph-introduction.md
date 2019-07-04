---
title: Azure Cosmos DB Gremlin API'sine giriş
description: Düşük gecikme süresine sahip yoğun grafikleri Apache TinkerPop’un Gremlin grafik sorgu dilini kullanarak depolamak, sorgulamak ve dolaştırmak için Azure Cosmos DB’yi nasıl kullanabileceğinizi öğrenin.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 06/25/2019
ms.author: lbosq
ms.openlocfilehash: 126c825106b7844a5fc8a5a3cdbcc7aa6c273b5b
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502798"
---
# <a name="introduction-to-azure-cosmos-db-gremlin-api"></a>Azure Cosmos DB’ye giriş: Gremlin API

[Azure Cosmos DB](introduction.md) görev açısından kritik uygulamalar için Microsoft'un Global olarak dağıtılmış çok modelli veritabanı hizmeti. Bu, çok modelli bir veritabanıdır ve belge, anahtar-değer, grafik ve sütunlu veri modellerini destekler. Azure Cosmos DB Gremlin API depolamak ve graf verileri dilediğiniz ölçekte bir tam olarak yönetilen veritabanı ortamı ile çalıştırmak için kullanılır.  

![Azure Cosmos DB grafik mimarisi](./media/graph-introduction/cosmosdb-graph-architecture.png)

Bu makale, Azure Cosmos DB Gremlin API'ye genel bir bakış sağlar ve milyarlarca köşesi ve kenarı olan yoğun grafikleri depolamak için bunu nasıl kullanabileceğinizi açıklar. Milisaniyelik gecikme garantisiyle grafik sorgu ve grafik yapısı bir kolayca geliştirmek. Azure Cosmos DB Gremlin API'si temel [Apache TinkerPop](https://tinkerpop.apache.org) standart grafik veritabanı ve kullanımları Gremlin sorgu dili. 

Azure Cosmos DB Gremlin API'si, esneklik ve ilişkisel yaklaşımları eksikliği ile ilişkili en yaygın veri sorunlar için benzersiz, esnek bir çözüm sağlamak için yüksek düzeyde ölçeklenebilir ve yönetilebilir bir altyapı graf veritabanı algoritmalarının gücünü birleştirir. 

## <a name="features-of-azure-cosmos-db-graph-database"></a>Azure Cosmos DB grafik veritabanının özellikleri
 
Azure Cosmos DB; genel dağıtım, depolama ve aktarım hızında esnek ölçeklendirme, otomatik dizinleme ve sorgu, ayarlanabilir tutarlılık düzeyleri ve TinkerPop standardı desteği sunan, tam olarak yönetilen bir grafik veritabanıdır. 

Azure Cosmos DB Gremlin API sunan fark yaratan özellikleri şunlardır:

* **Esnek bir şekilde ölçeklenebilir aktarım hızı ve depolama**

  Gerçek dünyadaki grafiklerin, tek bir sunucunun kapasitesinin yetmeyeceği biçimde ölçeklendirilmesi gerekir. Azure Cosmos DB açısından neredeyse sınırsız boyutu ve sağlanan aktarım hızı yatay olarak ölçeklenebilen graf veritabanları destekler. Graf veritabanı ölçeği büyüdükçe, veriyi otomatik olarak kullanarak dağıtılacak [grafik bölümleme](https://docs.microsoft.com/azure/cosmos-db/graph-partitioning).

* **Çok bölgeli çoğaltma**

  Azure Cosmos DB graph verilerinizi herhangi bir Azure bölgesine otomatik olarak çoğaltma yapabilirsiniz. Çoğaltma verilere genel erişim gerektiren uygulamaların geliştirilmesini kolaylaştırır. Azure Cosmos DB, okuma gecikme süresi en aza ek olarak, nadir bir bölgede bir hizmet kesintisi durumunda uygulamanızın sürekliliğini sağlamak bir bölgesel yük devretme mekanizması sağlar. 

* **Hızlı sorgular ve çapraz geçişleri en yaygın olarak benimsenen grafik sorgu standart**

  Heterojen köşeleri ve kenarları depolayın ve bildiğiniz bir Gremlin söz dizimiyle bu belgeleri sorgulayın. Gremlin graf algoritmaları ortak uygulamak için zengin bir arabirim sunan bir kesinlik temelli, işlevsel bir sorgu dilidir. 
  
  Azure Cosmos DB, gerçek zamanlı zengin sorgulara ve çapraz geçişleri şema ipuçları, ikincil dizinler veya görünümler belirtmek zorunda kalmadan sağlar. Daha fazla bilgi için bkz. [Gremlin kullanarak sorgu grafikleri](gremlin-support.md).

* **Tam olarak yönetilen bir grafik veritabanı**

  Azure Cosmos DB, veritabanı ve makine kaynaklarını yönetme ihtiyacını ortadan kaldırır. Çoğu varolan grafik veritabanı platformlarına, yaptıkları altyapı sınırlarına bağlı ve genellikle yüksek derecede Bunun çalışmasını sağlamak için bakım gerektirir. 
  
  Tam olarak yönetilen bir Microsoft Azure hizmeti olduğundan sanal makineleri yönetme, çalışma zamanı yazılım güncelleştirmesi, parçalama ya da çoğaltma yönetmek veya karmaşık veri katmanı yükseltmeleriyle uğraşmanız gerek yoktur. Tüm grafikler otomatik olarak yedeklenir ve bölgesel arızalara karşı korunur. Bu garanti eder, geliştiricilerin ve veritabanlarını yönetmek yerine uygulama değer sunmaya odaklanma sağlar. 

* **Otomatik dizin oluşturma**

  Azure Cosmos DB, varsayılan olarak grafiğin düğümlerindeki ve kenarlarındaki tüm özelliklerin otomatik olarak dizinini oluşturur ve herhangi bir şemayı ya da ikincil dizinlerin oluşturulmasını beklemez veya gerektirmez. Daha fazla bilgi edinin [Azure Cosmos DB'de dizinleme](https://docs.microsoft.com/azure/cosmos-db/index-overview). 

* **Apache TinkerPop ile uyumluluk**

  Azure Cosmos DB destekler [açık kaynaklı Apache TinkerPop standart](http://tinkerpop.apache.org/). Tinkerpop standart uygulamaları ve Azure Cosmos DB Gremlin API'si ile kolayca tümleştirilebilir kitaplıkları geniş bir ekosistemi vardır. 

* **İnce ayarlanabilir tutarlılık düzeyleri**

  Tutarlılık ve performans arasında en iyi dengeyi elde etmek için iyi tanımlanmış beş tutarlılık düzeyi arasından seçim yapın. Azure Cosmos DB sorgular ve okuma işlemleri için beş farklı tutarlılık düzeyi sunar: güçlü, sınırlanmış eskime durumu, oturum, tutarlı ön ek ve son. Bu ayrıntılı ve iyi tanımlanmış tutarlılık düzeyleri tutarlılık, kullanılabilirlik ve gecikme süresi arasında sağlam bir denge kurmanıza olanak sağlar. [Azure Cosmos DB'deki ayarlanabilir tutarlılık düzeyleri](consistency-levels.md) sayfasına giderek daha fazla bilgi edinin.

## <a name="scenarios-that-can-use-gremlin-api"></a>Gremlin API kullanabilen senaryolar
Azure Cosmos DB’nin grafik desteğinin kullanılabileceği bazı senaryolar aşağıda verilmiştir:

* Sosyal ağlar

  Müşterileriniz ve diğer kişilerle etkileşimleri hakkındaki verileri birleştirerek kişiselleştirilmiş deneyimleri geliştirebilir, müşteri davranışını tahmin edebilir veya kişileri benzer ilgi alanlarına sahip başkalarına bağlayabilirsiniz. Azure Cosmos DB, sosyal ağları yönetmek ve müşteri tercihlerini ve verilerini izlemek için kullanılabilir.

* Öneri altyapıları

  Bu senaryo, perakende sektöründe yaygın olarak kullanılır. Ürünler, kullanıcılar ve satın alma, gözatma veya bir öğeyi derecelendirme gibi kullanıcı etkileşimleri hakkındaki bilgileri birleştirerek özelleştirilmiş öneriler oluşturabilirsiniz. Azure Cosmos DB’nin düşük gecikme süresi, esnek ölçek ve yerel grafik desteği, bu etkileşimleri modelleme için idealdir.

* Jeo-uzamsal

  Telekomünikasyon, lojistik ve seyahat planlama alanlarındaki birçok uygulamanın, bir bölgede ilgi çekici bir konum bulması veya iki konum arasındaki en kısa/en uygun yolu belirlemesi gerekir. Azure Cosmos DB, bu sorunları için idealdir.

* Nesnelerin İnterneti

  IOT cihazları arasındaki ağ ve bağlantılar bir grafik olarak modellendiğinde, cihazlarınızın ve varlıklarınızın durumunu daha iyi kavrayabilirsiniz. Ayrıca ağın bir bölümündeki değişikliklerin başka bir bölümü nasıl etkileyebileceğini de öğrenebilirsiniz.

## <a name="introduction-to-graph-databases"></a>Graf veritabanları giriş
Gerçek dünyada görünen veriler doğal olarak bağlıdır. Geleneksel veri modelleme ayrı ayrı varlıklar tanımlayan ve aralarındaki ilişkiler zamanında bilgi işlem odaklanır. Bu model, kendine göre avantajları olduğundan, altında kendi kısıtlamaları yönetmek için sahip üst düzeyde bağlantılı veri zor olabilir.  

Bir grafik veritabanı yaklaşımı kalıcı depolama katmanı ilişkilerde bunun yerine, üst düzeyde verimli graf alma işlemleri için müşteri adayları kullanır. Azure Cosmos DB Gremlin API'si destekler [özelliği grafik modeli](https://tinkerpop.apache.org/docs/current/reference/#intro).

### <a name="property-graph-objects"></a>Özellik grafik nesneleri

Bir özellik [grafik](http://mathworld.wolfram.com/Graph.html) oluşur yapısıdır [köşeler](http://mathworld.wolfram.com/GraphVertex.html) ve [kenarlar](http://mathworld.wolfram.com/GraphEdge.html). Her iki nesnenin özellikleri olarak anahtar-değer çiftleri tercihe bağlı sayıda olabilir. 

* **Köşe** -köşeler bir kişi, yer veya olay gibi farklı varlıkları gösterir.

* **Kenarlar** - Kenarlar, köşeler arasındaki ilişkileri gösterir. Bir kişinin başka bir kişiyi tanıması, bir etkinliğe katılması veya kısa süre önce bir konumda bulunması bağlantılar buna örnek gösterilebilir. 

* **Özellikler** - Özellikler, köşeler ve kenarlar hakkında bilgi ifade eder. Köşe veya kenarları özelliklerinde herhangi bir sayıda olabilir ve açıklamak ve sorgu nesneleri filtrelemek için kullanılabilir. Örnek özellikleri ada ve yaş sahip bir köşe veya zaman damgası ve/veya ağırlık olabilir bir kenar içerir. 

Graf veritabanları genellikle NoSQL dahilinde yer alan ve ilişkisel olmayan, bir şema veya kısıtlanmış veri modeli üzerinde hiçbir bağımlılık olduğundan veritabanı kategorisi. Bu şema eksikliği modelleme ve doğal olarak ve verimli bir şekilde bağlı yapıları Depolama olanağı sağlar. 

### <a name="gremlin-by-example"></a>Örneğe göre Gremlin
Sorguların Gremlin’de nasıl ifade edildiğini anlamak için örnek bir grafik kullanalım. Aşağıdaki şekilde kullanıcılar, ilgi alanları ve cihazlar hakkındaki verileri yöneten bir iş uygulaması grafik biçiminde gösterilir.  

![Kişileri, cihazları ve ilgi alanlarını gösteren örnek grafik](./media/gremlin-support/sample-graph.png) 

Bu grafikte aşağıdaki köşe türleri (Gremlin’de “etiket” olarak adlandırılır) vardır:

- Kişiler: Grafiği bir kez deneme Thomas ve Ben üç kişi vardır
- İlgi alanları: Bu örnekte, oyunu futbol kendi ilgi alanlarına
- Cihazlar: Kullanımı cihazlar
- İşletim sistemleri: Cihaz üzerinde çalışan işletim sistemleri

Aşağıdaki kenar türleri/etiketleri üzerinden bu varlıklar arasındaki ilişkiyi temsil ediyoruz:

- Bilir: Örneğin, "Thomas Robin bilir"
- İster misiniz: Bizim Graph'te kişiler, örneğin, "Ben futbol ilgilenmektedir" ilgi alanları göstermek için
- RunsOS: Dizüstü bilgisayar Windows işletim sistemi çalıştırır.
- Kullanır: Bir kişi hangi cihaz göstermek için kullanır. Örneğin Robin, seri numarası 77 olan bir Motorola telefon kullanır

Şimdi [Gremlin Console](https://tinkerpop.apache.org/docs/3.3.2/reference/#gremlin-console)’u kullanarak bu grafiğe yönelik birkaç işlem yapalım. Dilerseniz bu işlemleri, tercih ettiğiniz platformdaki (Java, Node.js, Python veya .NET) Gremlin sürücülerini kullanarak da gerçekleştirebilirsiniz.  Azure Cosmos DB’de nelerin desteklendiğine bakmadan önce söz dizimine hakkında bilgi edinmek için birkaç örneğe bakalım.

İlk olarak CRUD’a bakalım. Aşağıdaki Gremlin deyimi “Thomas” köşesini grafiğe ekler:

```java
:> g.addV('person').property('id', 'thomas.1').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Daha sonra aşağıdaki Gremlin deyimi, Thomas ve Robin arasına bir “Tanıma” kenarı ekler.

```java
:> g.V('thomas.1').addE('knows').to(g.V('robin.1'))
```

Aşağıdaki sorgu, “kişiler” köşesini ilk adlarına göre azalan sırada döndürür:
```java
:> g.V().hasLabel('person').order().by('firstName', decr)
```

Grafiklerin asıl iyi olduğu kısımlar, “Thomas’ın arkadaşları hangi işletim sistemini kullanıyor?” gibi sorular sorduğunuzda ortaya çıkıyor. Grafikten ilgili bilgi almak için bu Gremlin geçişi çalıştırabilirsiniz:

```java
:> g.V('thomas.1').out('knows').out('uses').out('runsos').group().by('name').by(count())
```
Şimdi de Azure Cosmos DB’nin Gremlin geliştiricilerine neler sunduğuna bakalım.

## <a name="next-steps"></a>Sonraki adımlar
Azure Cosmos DB’de grafik desteği hakkında daha fazla bilgi için bkz.

* [Azure Cosmos DB grafik öğreticisi](create-graph-dotnet.md) ile çalışmaya başlayın.
* [Azure Cosmos DB’de Gremlin kullanarak grafikleri sorgulamayı](gremlin-support.md) öğrenin.
