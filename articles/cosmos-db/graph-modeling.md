---
title: Graf verileri için Azure Cosmos DB Gremlin API modelleme
description: Cosmos DB Gremlin API kullanımı bir grafik veritabanı modeli hakkında bilgi edinin.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 06/24/2019
ms.author: lbosq
ms.openlocfilehash: c6ae23efa90874bbefc2aff35f8798aa6c0da791
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503423"
---
# <a name="graph-data-modeling-for-azure-cosmos-db-gremlin-api"></a>Graf verileri için Azure Cosmos DB Gremlin API modelleme

Aşağıdaki belge, grafik veri modelleme öneriler sağlamak için tasarlanmıştır. Bu adım veri geliştikçe bir grafik veritabanı sisteminin performans ve ölçeklenebilirlik sağlamak için önemlidir. Verimli veri modeli ile büyük ölçekli grafikleri özellikle önemlidir.

## <a name="requirements"></a>Gereksinimler

Bu kılavuzda açıklanan işlemi aşağıdaki varsayımları temel alır:
 * **Varlıkları** sorun alanı tanımlanır. Bu varlıkların kullanılması amaçlanmaktadır _atomik olarak_ her istek için. Diğer bir deyişle, veritabanı sisteminin birden çok sorgu isteği tek bir varlığın veriyi almak için tasarlanmamıştır.
 * Bir anlayış yoktur **okuma ve yazma gereksinimlerini** veritabanı sistemi. Bu gereksinimler, grafik veri modeli için gereken en iyi duruma getirme yönlendirecektir.
 * İlkeleri [Apache Tinkerpop özelliği grafik standart](http://tinkerpop.apache.org/docs/current/reference/#graph-computing) de anlaşılabilir.

## <a name="when-do-i-need-a-graph-database"></a>Bir grafik veritabanı ne gerekir?

Varlıklar ve ilişkiler veri etki alanındaki herhangi biri aşağıdaki özelliklere sahip, bir grafik veritabanı çözümü en uygun şekilde uygulanabilir: 

* Varlıklar **yüksek oranda bağlı** açıklayıcı ilişkileri üzerinden. Bu senaryoda avantajı ilişkileri depolamada kalıcı gerçeğidir.
* Vardır **döngüsel ilişkiler** veya **varlıklar'kendi kendine başvuru**. Bu düzen, genellikle ilişkisel kullanırken veya veritabanları belge kolay değildir.
* Vardır **dinamik olarak ilişkiler gelişen** varlıklar arasında. Bu düzen, birçok düzeyleri hiyerarşik veya ağaç yapılandırılmış verilerle özellikle geçerlidir.
* Vardır **çoktan çoğa ilişkiler** varlıklar arasında.
* Vardır **varlıkları ve ilişkileri gereksinimleri okunup yazılacağını**. 

Yukarıdaki ölçütler karşılandığında, büyük olasılıkla bir grafik veritabanı yaklaşım için avantaj sağlayacaktır **sorgu karmaşıklığı**, **veri modeli ölçeklenebilirlik**, ve **sorgu performansına**.

Sonraki adım, grafik işlem ya da analitik amaçlar için kullanılmak üzere geçiyor, belirlemektir. Graf yoğun bir hesaplama ve veri işleme iş yükleri için kullanılmak üzere tasarlanmıştır, değer keşfetmek için olurdu [Cosmos DB Spark Bağlayıcısı](https://docs.microsoft.com/azure/cosmos-db/spark-connector) ve kullanımını [GraphX Kitaplığı](https://spark.apache.org/graphx/). 

## <a name="how-to-use-graph-objects"></a>Graf nesneleri kullanma

[Apache Tinkerpop özelliği grafik standart](http://tinkerpop.apache.org/docs/current/reference/#graph-computing) iki nesne türlerini tanımlar **köşeler** ve **kenarlar**. 

Graf nesneleri özellikleri için en iyi uygulamalar şunlardır:

| Object | Özellik | Tür | Notlar |
| --- | --- | --- |  --- |
| Vertex | id | String | Bölüm başına benzersiz olarak zorlanır. Ekleme sırasında sağlanan ve otomatik olarak oluşturulan bir değer değil GUID barındırılacaktır. |
| Vertex | label | String | Bu özellik, köşe temsil eden varlık türünü tanımlamak için kullanılır. Bir değer belirtilirse, bu değilse, varsayılan değer "köşe" kullanılır. |
| Vertex | properties | Dize, Boolean, sayısal | Her köşe, anahtar-değer çiftleri olarak depolanan ayrı özelliklerin listesi. |
| Vertex | Bölüm anahtarı | Dize, Boolean, sayısal | Bu özellik, köşe ve kenarlarını giden depolanacağı tanımlar. Daha fazla bilgi edinin [grafik bölümleme](graph-partitioning.md). |
| Edge | id | String | Bölüm başına benzersiz olarak zorlanır. Otomatik olarak oluşturulan varsayılan olarak. Kenarlar genellikle benzersiz bir kimliğe göre alınmasına gerek yok |
| Edge | label | String | Bu özellik, iki köşe sahip ilişki türünü tanımlamak için kullanılır. |
| Edge | properties | Dize, Boolean, sayısal | Her edge'de anahtar-değer çiftleri olarak depolanan ayrı özelliklerin listesi. |

> [!NOTE]
> Değerini, kendi kaynak köşe göre otomatik olarak atanır aldığından kenarlar bir bölüm anahtarı değeri gerektirmez. Daha fazla bilgi [grafik bölümleme](graph-partitioning.md) makalesi.

## <a name="entity-and-relationship-modeling-guidelines"></a>Varlık ve ilişki modelleme Kılavuzu

Bir dizi için bir Azure Cosmos DB Gremlin API grafik veritabanı modelleme yaklaşımı veri yönergeleri verilmiştir. Bu yönergeler, veri etki alanı ve sorgular için mevcut bir tanımı olduğunu varsayın.

> [!NOTE]
> Aşağıda özetlenen adımlar, öneriler olarak gösterilir. Son modelin değerlendirilir ve üretime hazır olarak kendi göz önünde bulundurarak önce test. Ayrıca, aşağıdaki önerileri Azure Cosmos DB Gremlin API uygulamasına özgüdür. 

### <a name="modeling-vertices-and-properties"></a>Köşe ve özellikleri modelleme 

Grafik veri modeli için ilk adımı için tanımlanan her varlık eşlemektir bir **köşe nesne**. Bir bire bir eşleme köşeler için tüm varlıkların bir ilk adım ve değiştirmek için konu olmalıdır.

Ortak bir durumu ayrı köşeler tek bir varlık özelliklerini eşlemektir. Burada, aynı varlık iki farklı şekilde temsil edilir aşağıdaki örnekte, göz önünde bulundurun:

* **Köşe tabanlı özellikleri**: Bu yaklaşımda, varlık özelliklerini tanımlamak için bu üç ayrı köşe ve kenarlar iki kullanır. Bu yaklaşım, yedeklilik azaltabilir, ancak modeli karmaşıklığı artırır. Model karmaşıklık arasında bir artış, gecikme süresi, sorgu karmaşıklığı ve maliyeti hesaplama neden olabilir. Bu model, bölümleme zorluklarından de sunabilir.

![Varlık modeli köşeler özellikleri ile.](./media/graph-modeling/graph-modeling-1.png)

* **Köşe özelliğini Katıştırılmış**: Bu yaklaşım bir köşe içinde varlığın tüm özellikleri temsil etmek için anahtar-değer çifti listenin yararlanır. Bu yaklaşım daha basit sorgular ve daha fazla maliyet açısından verimli çapraz geçişleri önünü açacak azaltılmış modeli karmaşıklığı sağlar.

![Varlık modeli köşeler özellikleri ile.](./media/graph-modeling/graph-modeling-2.png)

> [!NOTE]
> Yukarıdaki örneklerde, yalnızca varlık özellikleri bölerek iki yolu karşılaştırması gösterecek şekilde basitleştirilmiş bir grafik modeli gösterir.

**Özelliğini Katıştırılmış köşeler** deseni genellikle sağlar bir daha fazla yüksek performanslı ve ölçeklenebilir bir yaklaşım. Yeni bir grafik veri modeli varsayılan yaklaşımı, bu düzenine odaklanacak biçimde gravitate.

Ancak, bir özelliğe başvurma avantajları burada sağlayabilir senaryo vardır. Örneğin: başvurulan özelliği sık güncelleştirildiyse. Sürekli olarak değişen bir özellik temsil etmek için ayrı bir köşe kullanarak güncelleştirme gerektiren yazma işlemlerini en aza.

### <a name="relationship-modeling-with-edge-directions"></a>İlişki uç yönergeleri ile modelleme

Köşe modellenir sonra kenarlarını onlar arasındaki ilişkileri göstermek için eklenebilir. Değerlendirilecek gereken ilk nedeniyle **ilişkinin yönünü**. 

Edge nesneleri tarafından bir geçişi kullanırken izleyen bir varsayılan yönüne sahip `out()` veya `outE()` işlevi. Tüm köşeleri giden kenarlarına depolandığından kullanarak doğal bu yönü, verimli bir işlem içinde sonuçlanır. 

Ancak, bir kenar ters yönde geçiş yapma, kullanarak `in()` işlev, her zaman bir bölümler arası sorgu neden olur. Daha fazla bilgi edinin [grafik bölümleme](graph-partitioning.md). Sürekli olarak kullanarak çapraz geçiş yapma gereksinimi varsa `in()` işlevi önerilir her iki yönde de kenarlar ekleme.

Edge yönü kullanarak belirleyebilirsiniz `.to()` veya `.from()` için doğrulamaları `.addE()` Gremlin adımı. Kullanarak veya [Gremlin API Bulkexecutor'a kitaplığının](bulk-executor-graph-dotnet.md).

> [!NOTE]
> Edge nesneleri, varsayılan olarak bir yön vardır.

### <a name="relationship-labeling"></a>İlişki etiketleme

Açıklayıcı ilişki etiketleri kullanarak kenar çözümleme işlemlerinin verimliliğini artırabilir. Bu düzen aşağıdaki yollarla uygulanabilir:
* Bir ilişki etiketlemek için genel olmayan terimleri kullanın.
* Hedef köşe etiketine kaynak köşe etiketini ilişki adı ile ilişkilendirin.

![Örnekleri etiketleme ilişki.](./media/graph-modeling/graph-modeling-3.png)

Daha fazla belirli traverser kenarlar daha iyi filtrelemek için kullanacağı etiketi. Bu karar, sorgu maliyeti de önemli bir etkisi olabilir. Herhangi bir zamanda sorgu maliyeti değerlendirebilirsiniz [executionProfile adım kullanarak](graph-execution-profile.md).


## <a name="next-steps"></a>Sonraki adımlar: 
* Kullanıma alma listesi desteklenen [Gremlin adımı](gremlin-support.md).
* Hakkında bilgi edinin [veritabanı bölümleme graph](graph-partitioning.md) ile büyük ölçekli grafikleri dağıtılacak.
* Kullanarak, Gremlin sorguları değerlendirmek [yürütme profilini adım](graph-execution-profile.md).
