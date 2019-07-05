---
title: Azure Cosmos DB Gremlin API veri bölümleme
description: Bölümlenmiş bir grafik Azure Cosmos DB'de nasıl kullanabileceğinizi öğrenin. Bu makalede ayrıca gereksinimleri ve bölümlenmiş bir grafik için en iyi uygulamaları açıklar.
author: luisbosquez
ms.author: lbosq
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: conceptual
ms.date: 06/24/2019
ms.custom: seodec18
ms.openlocfilehash: 4c8761d82c8a735ac9c4bff2e5ac0107b2a57fe0
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537545"
---
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Bölümlenmiş bir grafik Azure Cosmos DB içinde kullanma

Azure Cosmos DB Gremlin API anahtar özelliklerini de yatay ölçeklendirme aracılığıyla büyük ölçekli grafikleri işlemek yeteneğidir. Kapsayıcıları, depolama ve aktarım hızı bakımından bağımsız olarak ölçeklendirebilirsiniz. Azure Cosmos DB'de graf verilerini depolamak için otomatik olarak ölçeklendirilebilir kapsayıcılar oluşturabilirsiniz. Verileri otomatik olarak bağlı olarak belirtilen dengelenir **bölüm anahtarı**.

**Bölümleme gerekli** kapsayıcı boyutu 10 GB'den fazla depolamak için bekleniyorsa ya da 10. 000'den fazla istek birimi (RU) saniyede ayırmak istiyorsanız. Aynı ilkeler gelen [Azure Cosmos DB bölümleme mekanizması](partition-data.md) aşağıda açıklanan birkaç grafik özgü iyileştirmeler ile geçerlidir.

![Grafik bölümleme.](./media/graph-partitioning/graph-partitioning.png)

## <a name="graph-partitioning-mechanism"></a>Grafik bölümleme mekanizması

Aşağıdaki yönergeler, Azure Cosmos DB'de bölümleme stratejisini nasıl çalıştığı açıklanmaktadır:

- **Köşe ve kenarlar hem JSON belgeleri olarak depolanır**.

- **Köşe gerektiren bir bölüm anahtarı**. Bu anahtar, hangi bölüme bir karma algoritma köşe depolanacak belirler. Bölüm anahtar özellik adı, yeni bir kapsayıcı oluşturulurken tanımlanır ve bir biçime sahiptir: `/partitioning-key-name`.

- **Kenarlar ile kendi kaynak köşe depolanacağı**. Diğer bir deyişle, her köşe için bölüm anahtarını giden kenarlarını birlikte depolandıkları tanımlar. Bu iyileştirme, bölümler arası sorgular kullanırken önlemek için yapılır `out()` graf sorgularını kardinalitesini.

- **Kenarlar, bunların işaret köşeler başvurular içerir**. Tüm kenarları bölüm anahtarları ve işaret ettikleri köşelerin kimlikleri depolanır. Tüm bu hesaplama yapar `out()` yönü sorguları her zaman kapsamlı bölümlenmiş bir sorgu ile görme bölümler arası sorgu değil olmalıdır. 

- **Bir bölüm anahtarı belirtmeniz gerekir graf sorgularını**. Tek bir köşe seçildiğinde mümkün olduğunda yatay Azure Cosmos DB'de bölümleme tam avantajından yararlanmak için bölüm anahtarı belirtilmesi gerekir. Bölümlenmiş bir grafikte bir veya daha fazla köşe seçmeye yönelik sorgular şunlardır:

    - `/id` ve `/label` Gremlin API içinde bir kapsayıcı için bölüm anahtarı olarak desteklenmez.


    - Ardından Kimliğe göre bir köşe seçerek **kullanarak `.has()` bölüm anahtar özelliği belirtmek için adım**: 
    
        ```java
        g.V('vertex_id').has('partitionKey', 'partitionKey_value')
        ```
    
    - Bir köşe tarafından seçilmesi **bölüm anahtarı değeri ve kimliği de dahil olmak üzere bir tanımlama grubu belirterek**: 
    
        ```java
        g.V(['partitionKey_value', 'vertex_id'])
        ```
        
    - Belirten bir **diziler bölüm anahtarı değerlerine ve kimlikler dizisi**:
    
        ```java
        g.V(['partitionKey_value0', 'verted_id0'], ['partitionKey_value1', 'vertex_id1'], ...)
        ```
        
    - Köşe kimlikleri kümesi seçme ve **bölüm anahtar değerlerin bir listesini belirtme**: 
    
        ```java
        g.V('vertex_id0', 'vertex_id1', 'vertex_id2', …).has('partitionKey', within('partitionKey_value0', 'partitionKey_value01', 'partitionKey_value02', …)
        ```

    - Kullanarak **bölümleme stratejisi** başında bir sorgunun ve Gremlin sorgu geri kalanı kapsamı için bir bölüm belirtme: 
    
        ```java
        g.withStrategies(PartitionStrategy.build().partitionKey('partitionKey').readPartitions('partitionKey_value').create()).V()
        ```

## <a name="best-practices-when-using-a-partitioned-graph"></a>Bölümlenmiş bir grafik kullanırken en iyi yöntemler

Sınırsız kapsayıcılar ile bölümlenmiş grafiklerini kullanarak, performans ve ölçeklenebilirlik sağlamak için aşağıdaki yönergeleri kullanın:

- **Her zaman bir köşe sorgulanırken bölüm anahtarı değeri belirtin**. Köşe bilinen bir bölümün dışında alma performans elde etmek için bir yoldur. Kenarlar, hedef köşeler için başvuru kimliği ve bölüm anahtarı içeren olduğundan tüm sonraki bitişik işlemler her zaman bir bölüme kapsamı.

- **Mümkün olduğunda kenarlar sorgulanırken giden yön kullanın**. Yukarıda belirtildiği gibi kendi kaynak köşe giden yönde ile kenarlar depolanır. Bu nedenle bu düzendeki aklınızda sorgular ve verileri tasarlanırsa bölümler arası sorgular için maksimum olasılığını en aza indirilir. Tam, `in()` sorgu her zaman bir pahalı bir yaygın sorgu olacaktır.

- **Verileri bölümler arasında eşit şekilde dağıtacak bölüm anahtarı seçin**. Bu karar, çözümün veri modeli üzerinde yoğun bir şekilde bağlıdır. Bir uygun bölüm anahtarı oluşturma hakkında daha fazla [bölümleme ve ölçeklendirme Azure Cosmos DB'de](partition-data.md).

- **Bir bölüm sınırları içinde verileri almak için sorguları iyileştirmek**. Sorgulama desenleri için uygun bir bölümleme stratejisi hizalanabilir. Tek bölümden veri elde sorguları mümkün olan en iyi performansı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Ardından bu makaleleri okuyun geçebilirsiniz:

* Hakkında bilgi edinin [bölümleme ve ölçeklendirme Azure Cosmos DB'de](partition-data.md).
* Hakkında bilgi edinin [Gremlin API Gremlin desteği](gremlin-support.md).
* Hakkında bilgi edinin [Gremlin API giriş](graph-introduction.md).
