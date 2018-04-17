---
title: Grafik API'si bölümleme | Microsoft Docs
description: Bölümlenmiş bir grafik Azure Cosmos DB'de nasıl kullanabileceğiniz hakkında bilgi edinin.
services: cosmos-db
author: luisbosquez
documentationcenter: ''
manager: kfile
ms.assetid: ''
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/28/2018
ms.author: lbosq
ms.openlocfilehash: db41efeee467d2cda89f62e0a18cf89cec2d9e63
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Bölümlenmiş bir grafik Azure Cosmos DB içinde kullanma

Grafik API'si Azure Cosmos veritabanı'nın temel özellikleri de yatay ölçeklenebilirlik aracılığıyla büyük ölçekli grafikleri işlemek yeteneğidir. Bu işlem aracılığıyla elde edilen [Azure Cosmos veritabanı özellikleri bölümleme](partition-data.md#how-does-partitioning-work), hangi depolama ve işleme açısından bağımsız olarak ölçeklendirebilirsiniz kapsayıcıları, olarak da adlandırılan koleksiyonların kullanın. Azure Cosmos DB tüm API'leri kapsayıcıları aşağıdaki türlerini destekler:

- **Koleksiyon sabit**: Bu koleksiyonları bir grafik depolayabilir boyutu en fazla 10.000 istek birimleri ayrılmış saniye başına en fazla 10 GB veritabanı. Sabit bir koleksiyon oluşturmak için bir bölüm anahtarı özelliği verileri belirtmek gerekli değildir.

- **Sınırsız koleksiyonu**: Bu koleksiyonları otomatik olarak bir grafik yatay bölümleme aracılığıyla 10 GB sınırından depolamak için ölçeklendirebilirsiniz. Her bölüm 10 GB depolar ve veriler otomatik olarak temel alınarak dengelenir **belirtilen bölüm anahtarı**, sınırsız bir koleksiyonu kullanırken gerekli parametre olacak. Bu tür bir kapsayıcı veya saniye başına en fazla 100.000 istek birimleri izin verebilir ve neredeyse sınırsız veri boyutu depolayabilirsiniz [Destek'e başvurarak](https://aka.ms/cosmosdbfeedback?subject=Cosmos%20DB%20More%20Throughput%20Request).

Bu belgede, hem köşeleri (veya düğümler) için kendi uygulamaları ile birlikte nasıl grafik veritabanları bölümlenir üzerinde özellikleri açıklanacaktır ve kenarları.

## <a name="requirements-for-partitioned-graph"></a>Bölümlenmiş grafik gereksinimleri

Bölümlenmiş grafik kapsayıcı oluştururken anlaşılması gereken Ayrıntılar verilmiştir:
- **Bölümleme yukarı ayarı gerekli olacak** koleksiyon birden fazla 10 GB boyutunda olması bekleniyorsa ve/veya 10. 000'den fazla istek birimleri (RU/s) saniyede ayırdıktan gerekli olursa.
- **Köşeleri ve kenarları JSON belgeleri olarak depolanır** Azure Cosmos DB grafik API'sini kapsayıcısının arka uçtaki.
- **Köşeleri gerektiren bir bölüm anahtarı**. Bu anahtar, karma algoritma üzerinden hangi bölümünde köşe depolanacak belirler. Bu bölüm anahtarı adı bir boşluk veya özel karakterler olmadan Tek sözcüklü dizedir ve biçimini kullanarak yeni bir koleksiyon oluşturulurken tanımlanan `/partitioning-key-name` Portal.
- **Kenarlar ile bunların kaynak köşe depolanacağı**. Diğer bir deyişle, her köşe için bunların yanı sıra, giden kenarları depolanacağı bölüm anahtarını tanımlayacaksınız. Bu çapraz bölüm sorguları kullanırken önlemek için yapılır `out()` grafik sorgularda önem düzeyi.
- **Bölüm anahtarı belirtmeniz gerekir grafik sorguları**. Mümkün olduğunda tek bir köşe seçili olduğunda Azure Cosmos DB'de yatay bölümleme tam avantajından yararlanmak için bölüm anahtarı belirtilmelidir. Bölümlenmiş bir grafikte bir veya birden çok köşeleri seçmek için sorgular şunlardır:

    - Kimliğe göre köşe seçilerek **kullanarak `.has()` bölüm anahtar özelliği belirtmek için adım**: 
    
        ```
        g.V('vertex_id').has('partitionKey', 'partitionKey_value')
        ```
    
    - Köşe tarafından seçme **bölüm anahtarı değerini ve kimliği dahil olmak üzere bir tanımlama grubu belirterek**: 
    
        ```
        g.V(['partitionKey_value', 'vertex_id'])
        ```
        
    - Belirten bir **bölüm anahtarı değerlerini kimlikleri ve diziler dizisi**:
    
        ```
        g.V(['partitionKey_value0', 'verted_id0'], ['partitionKey_value1', 'vertex_id1'], ...)
        ```
        
    - Köşeleri kümesi seçme ve **bölüm anahtar değerlerinin bir listesini belirtme**: 
    
        ```
        g.V('vertex_id0', 'vertex_id1', 'vertex_id2', …).has('partitionKey', within('partitionKey_value0', 'partitionKey_value01', 'partitionKey_value02', …)
        ```

## <a name="best-practices-when-using-a-partitioned-graph"></a>Bölümlenmiş bir grafik kullanırken en iyi uygulamalar

Bölümlenmiş grafikleri sınırsız koleksiyonlarda kullanırken en verimli performans ve ölçeklenebilirlik sağlamak için izlenmesi gereken yönergeler şunlardır:
- **Her zaman bir köşe sorgulanırken bölüm anahtarı değerini belirtmeniz**. Bilinen bir bölümün dışında bir köşe alma, performans açısından en etkili yoldur.
- **Mümkün olduğunda kenarları sorgulanırken giden yön kullanmak**. Yukarıda belirtildiği gibi kendi kaynak köşeleri giden yön ile kenarları depolanır. Başka bir deyişle, sorgular ve veri aklınızda Bu düzendeki tasarlanmış çapraz bölüm sorgular için yeniden ayırma olasılığını en aza indirilir.
- **Veri bölümleri arasında eşit olarak dağıtır bir bölüm anahtarı seçin**. Bu kararı çözümü veri modelinin yoğun bir şekilde bağlıdır. Bir uygun bölüm anahtarı oluşturma hakkında daha fazla okuma [Partitining ve Azure Cosmos veritabanı ölçek](partition-data.md).
- **Mümkün olduğunda bir bölüm sınırları içinde verileri almak için sorguları en iyi duruma getirme**. En iyi bölümleme stratejisine sorgulanırken desenleri hizalı. Verileri tek bir bölümün dışında elde sorguları, olası en iyi performansı sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir genel bakış, kavramlar ve bir Azure Cosmos DB grafik API'si ile bölümleme için en iyi uygulamalar sağlandı. 

* Hakkında bilgi edinin [bölüm ve Azure Cosmos veritabanı ölçek](partition-data.md).
* Hakkında bilgi edinin [grafik API'si Gremlin desteği](gremlin-support.md).
* Hakkında bilgi edinin [grafik API'si giriş](graph-introduction.md).
