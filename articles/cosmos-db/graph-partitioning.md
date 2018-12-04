---
title: Azure Cosmos DB Gremlin API bölümleme
description: Bölümlenmiş bir grafik Azure Cosmos DB'de nasıl kullanabileceğinizi öğrenin.
services: cosmos-db
author: luisbosquez
ms.author: lbosq
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 27b0d9d7ca22ba346dbc288020f704dc7d27aa6a
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52837214"
---
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Bölümlenmiş bir grafik Azure Cosmos DB içinde kullanma

Azure Cosmos DB Gremlin API anahtar özelliklerini de yatay ölçeklenebilirlik aracılığıyla büyük ölçekli grafikleri işlemek yeteneğidir. Bu işlem aracılığıyla elde edilen [özellikleri Azure Cosmos DB'de bölümleme](partition-data.md), hangi depolama ve aktarım hızı bakımından bağımsız olarak ölçeklendirilebilir kapsayıcıların kullanın. Azure Cosmos DB aşağıdaki türden kapsayıcıya tüm API'leri destekler:

- **Sabit kapsayıcıyı**: Bu kapsayıcıların bir grafik depolayabilirsiniz 10.000 istek birimi / saniye için ayrılan maksimum boyutu 10 GB'a kadar veritabanı. Sabit bir kapsayıcı oluşturmak için bir bölüm anahtarı özelliği verileri belirtmek gerekli değildir.

- **Sınırsız sayıda kapsayıcı**: Bu kapsayıcıların bir grafiğin yatay bölümleme üzerinden 10 GB sınırından depolamak için otomatik olarak ölçeklendirebilirsiniz. Her bölüm 10 GB depolar ve verileri otomatik olarak bağlı olarak dengelenir **belirtilen bölüm anahtarı**, sınırsız bir kapsayıcıya kullanırken gerekli bir parametre olacak. Bu tür bir kapsayıcı veya saniye başına en fazla 100.000 istek birimleri izin verebilir ve neredeyse sınırsız veri boyutu depolayabilirsiniz [destek ile iletişim kurarak](https://aka.ms/cosmosdbfeedback?subject=Cosmos%20DB%20More%20Throughput%20Request).

Bu belgede, hem köşeler (veya düğümleri) için kendi uygulamaları ile birlikte grafik veritabanlarını nasıl bölümlenir üzerindeki özellikleri açıklanacaktır ve kenarlar.

## <a name="requirements-for-partitioned-graph"></a>Bölünmüş grafik gereksinimleri

Bölünmüş grafik kapsayıcısı oluştururken önce anlamanız gereken Ayrıntılar verilmiştir:

- **Bölümleme yedekleme ayarı gerekli olacak** kapsayıcı boyutu 10 GB'den fazla olması beklenir ve/veya 10. 000'den fazla istek birimi (RU/sn) saniyede ayırma gerekli olursa.

- **Köşe ve kenarlar hem JSON belgeleri olarak depolanır** arka uç bir Azure Cosmos DB Gremlin API kapsayıcısının içinde.

- **Köşe gerektiren bir bölüm anahtarı**. Bu anahtar, hangi bölüme bir karma algoritma köşe depolanacak belirler. Bu bölüm anahtarı adı boşluk veya özel karakter içermeyen bir Tek sözcüklü dizedir ve aşağıdaki biçimi kullanarak yeni bir kapsayıcı oluşturulurken tanımlanan `/partitioning-key-name` portalında.

- **Kenarlar ile kendi kaynak köşe depolanacağı**. Diğer bir deyişle, her köşe için bunların yanı sıra giden kenarlarını depolanacağı bölüm anahtarını tanımlayacaksınız. Bu bölümler arası sorgular kullanırken önlemek için yapılır `out()` graf sorgularını kardinalitesini.

- **Bir bölüm anahtarı belirtmeniz gerekir graf sorgularını**. Tek bir köşe seçildiğinde mümkün olduğunda yatay Azure Cosmos DB'de bölümleme tam avantajından yararlanmak için bölüm anahtarı belirtilmesi gerekir. Bölümlenmiş bir grafikte bir veya daha fazla köşe seçmeye yönelik sorgular şunlardır:

    - `/id` ve `/label` Gremlin API içinde bir kapsayıcı için bölüm anahtarı olarak desteklenmiyor...


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

Sınırsız kapsayıcılar bölümlenmiş grafiklerde kullanılırken en verimli performans ve ölçeklenebilirlik sağlamak için izlenmesi gereken yönergeler şunlardır:

- **Her zaman bir köşe sorgulanırken bölüm anahtarı değeri belirtin**. Bilinen bir bölümün dışında bir köşe edinme, performans açısından en verimli yoludur.

- **Mümkün olduğunda kenarlar sorgulanırken giden yön kullanın**. Yukarıda belirtildiği gibi kendi kaynak köşe giden yönde ile kenarlar depolanır. Bu bölümler arası sorgular için maksimum olasılığını unutmayın, bu düzendeki veriler ve sorgular tasarlanırsa küçültülür anlamına gelir.

- **Verileri bölümler arasında eşit şekilde dağıtacak bölüm anahtarı seçin**. Bu karar, çözümün veri modeli üzerinde yoğun bir şekilde bağlıdır. Bir uygun bölüm anahtarı oluşturma hakkında daha fazla [bölümleme ve ölçeklendirme Azure Cosmos DB'de](partition-data.md).

- **Mümkün olduğunda bir bölüm sınırları içinde verileri almak için sorguları iyileştirmek**. Sorgulama desenleri için uygun bir bölümleme stratejisi hizalanabilir. Tek bölümden veri elde sorguları mümkün olan en iyi performansı sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Bu makaledeki kavramları ve bir Azure Cosmos DB Gremlin API'si ile bölümleme için en iyi bir genel bakış sağlanmadı. 

* Hakkında bilgi edinin [bölümleme ve ölçeklendirme Azure Cosmos DB'de](partition-data.md).
* Hakkında bilgi edinin [Gremlin API Gremlin desteği](gremlin-support.md).
* Hakkında bilgi edinin [Gremlin API giriş](graph-introduction.md).
