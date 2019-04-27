---
title: Azure Cosmos DB Gremlin API veri bölümleme
description: Bölümlenmiş bir grafik Azure Cosmos DB'de nasıl kullanabileceğinizi öğrenin. Bu makalede ayrıca gereksinimleri ve bölümlenmiş bir grafik için en iyi uygulamaları açıklar.
author: rockboyfor
ms.author: v-yeche
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: conceptual
origin.date: 12/06/2018
ms.date: 03/18/2019
ms.custom: seodec18
ms.openlocfilehash: f1e486a302b440d819e15ef86f8d76ea5e50d201
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60888427"
---
<!--Verify sucessfully-->
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Bölümlenmiş bir grafik Azure Cosmos DB içinde kullanma

Azure Cosmos DB Gremlin API anahtar özelliklerini de yatay ölçeklendirme aracılığıyla büyük ölçekli grafikleri işlemek yeteneğidir. Yatay ölçeklendirme aracılığıyla gerçekleştirilir [özellikleri Azure Cosmos DB'de bölümleme](partition-data.md). Kapsayıcıları, depolama ve aktarım hızı bakımından bağımsız olarak ölçeklendirebilirsiniz. Azure Cosmos DB'de graf verilerini depolamak için otomatik olarak ölçeklendirilebilir kapsayıcılar oluşturabilirsiniz. Verileri otomatik olarak bağlı olarak belirtilen dengelenir **bölüm anahtarı**.

Bu belgede, hem köşeler (veya düğümleri) için kendi uygulamaları ile birlikte grafik veritabanlarını nasıl bölümlenir üzerindeki özellikleri açıklanacaktır ve kenarlar.

## <a name="requirements-for-partitioned-graph"></a>Bölünmüş grafik gereksinimleri

Bölünmüş grafik kapsayıcısı oluştururken önce anlamanız gereken Ayrıntılar verilmiştir:

- **Bölümleme gerekli** kapsayıcı boyutu 10 GB'den fazla depolamak için bekleniyorsa ya da 10. 000'den fazla istek birimi (RU) saniyede ayırmak istiyorsanız.

- **Köşe ve kenarlar hem JSON belgeleri olarak depolanır**.

- **Köşe gerektiren bir bölüm anahtarı**. Bu anahtar, hangi bölüme bir karma algoritma köşe depolanacak belirler. Bu bölüm anahtarı adı, boşluk veya özel karakter olmadan Tek sözcüklü bir dizedir. Bölüm anahtarı yeni bir kapsayıcı oluşturulurken tanımlanır ve bir biçime sahiptir: `/partitioning-key-name`.

- **Kenarlar ile kendi kaynak köşe depolanacağı**. Diğer bir deyişle, her köşe için bölüm anahtarını giden kenarlarını birlikte depolandıkları tanımlar. Bu bölümler arası sorgular kullanırken önlemek için yapılır `out()` graf sorgularını kardinalitesini.

- **Bir bölüm anahtarı belirtmeniz gerekir graf sorgularını**. Tek bir köşe seçildiğinde mümkün olduğunda yatay Azure Cosmos DB'de bölümleme tam avantajından yararlanmak için bölüm anahtarı belirtilmesi gerekir. Bölümlenmiş bir grafikte bir veya daha fazla köşe seçmeye yönelik sorgular şunlardır:

    - `/id` ve `/label` Gremlin API içinde bir kapsayıcı için bölüm anahtarı olarak desteklenmez.

    - Ardından Kimliğe göre bir köşe seçerek **kullanarak `.has()` bölüm anahtar özelliği belirtmek için adım**: 

        ```
        g.V('vertex_id').has('partitionKey', 'partitionKey_value')
        ```

    - Bir köşe tarafından seçilmesi **bölüm anahtarı değeri ve kimliği de dahil olmak üzere bir tanımlama grubu belirterek**: 

        ```
        g.V(['partitionKey_value', 'vertex_id'])
        ```

    - Belirten bir **diziler bölüm anahtarı değerlerine ve kimlikler dizisi**:

        ```
        g.V(['partitionKey_value0', 'verted_id0'], ['partitionKey_value1', 'vertex_id1'], ...)
        ```

    - Köşe kümesini seçme ve **bölüm anahtar değerlerin bir listesini belirtme**: 

        ```
        g.V('vertex_id0', 'vertex_id1', 'vertex_id2', …).has('partitionKey', within('partitionKey_value0', 'partitionKey_value01', 'partitionKey_value02', …)
        ```

## <a name="best-practices-when-using-a-partitioned-graph"></a>Bölümlenmiş bir grafik kullanırken en iyi yöntemler

Sınırsız kapsayıcılar ile bölümlenmiş grafiklerini kullanarak, performans ve ölçeklenebilirlik sağlamak için aşağıdaki yönergeleri kullanın:

- **Her zaman bir köşe sorgulanırken bölüm anahtarı değeri belirtin**. Köşe bilinen bir bölümün dışında alma performans elde etmek için bir yoldur.

- **Mümkün olduğunda kenarlar sorgulanırken giden yön kullanın**. Yukarıda belirtildiği gibi kendi kaynak köşe giden yönde ile kenarlar depolanır. Bu nedenle bu düzendeki aklınızda sorgular ve verileri tasarlanırsa bölümler arası sorgular için maksimum olasılığını en aza indirilir.

- **Verileri bölümler arasında eşit şekilde dağıtacak bölüm anahtarı seçin**. Bu karar, çözümün veri modeli üzerinde yoğun bir şekilde bağlıdır. Bir uygun bölüm anahtarı oluşturma hakkında daha fazla [bölümleme ve ölçeklendirme Azure Cosmos DB'de](partition-data.md).

- **Bir bölüm sınırları içinde verileri almak için sorguları iyileştirmek**. Sorgulama desenleri için uygun bir bölümleme stratejisi hizalanabilir. Tek bölümden veri elde sorguları mümkün olan en iyi performansı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Ardından bu makaleleri okuyun geçebilirsiniz:

* Hakkında bilgi edinin [bölümleme ve ölçeklendirme Azure Cosmos DB'de](partition-data.md).
* Hakkında bilgi edinin [Gremlin API Gremlin desteği](gremlin-support.md).
* Hakkında bilgi edinin [Gremlin API giriş](graph-introduction.md).

<!--Update_Description: new articles on  -->
<!--ms.date: 03/18/2019-->