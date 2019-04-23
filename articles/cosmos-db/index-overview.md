---
title: Azure Cosmos DB'yi dizine ekleme
description: Azure Cosmos DB'de dizinleme nasıl çalıştığını anlayın.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: thweiss
ms.openlocfilehash: 3bb8913725acf04f71aba8b4c4350235f2c44dfb
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59996739"
---
# <a name="indexing-in-azure-cosmos-db---overview"></a>Azure Cosmos DB - genel bakış dizin oluşturma

Azure Cosmos DB, şema veya dizin yönetimiyle ilgilenmenize gerek kalmadan uygulamanızı yineleme olanak tanıyan bir şemadan veritabanıdır. Varsayılan olarak, Azure Cosmos DB içindeki tüm öğeler için her bir özellik otomatik olarak dizinleyen, [kapsayıcı](databases-containers-items.md#azure-cosmos-containers) herhangi bir şema tanımlayın veya ikincil dizinler yapılandırmak zorunda kalmadan.

Bu makalenin amacı, Azure Cosmos DB verileri nasıl dizinler ve nasıl sorgu performansını artırmak için dizinleri kullandığı açıklayan sağlamaktır. Bu bölümde, nasıl özelleştirileceğini edinip gitmek için önerilen [dizinleme ilkeleri](index-policy.md).

## <a name="from-items-to-trees"></a>Öğeleri ağaçları

Her zaman bir öğe bir kapsayıcıda depolanır, içeriği bir JSON belgesi olarak öngörülen sonra bir ağaç gösterimine dönüştürülecek. Ne bu öğesinin her bir özellik, bir düğüm bir ağaç olarak gösterilen anlamına gelir. Sahte kök düğümü, bir üst öğenin tüm birinci düzey özellikleri olarak oluşturulur. Yaprak düğümleri bir öğe tarafından gerçekleştirilen gerçek skaler değerler içerir.

Örneğin, bu öğeyi göz önünde bulundurun:

    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 },
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }

Aşağıdaki ağaç tarafından temsil:

![Ağaç olarak temsil edilen önceki öğeye](./media/index-overview/item-as-tree.png)

Diziler ağacında nasıl kodlanmış unutmayın: dizi içinde giriş dizini ile etiketlenmiş bir ara düğümü bir dizideki her bir girdi alır (0, 1 vs.).

## <a name="from-trees-to-property-paths"></a>Özellik yolları ağaçlarından

Neden Azure Cosmos DB ağaçlara öğeleri dönüştürür. Bunun nedeni, yollarına ağaçların içinde tarafından başvurulabilmesi özellikler sağlayan olmasıdır. Bir özelliği olan yolu almak için biz ağaç kök düğümü aracılığıyla bu özelliğe geçiş ve geçilen her düğümün etiketleri birleştir.

Yukarıda açıklanan örnek öğesinden her bir özellik için yollar şunlardır:

    /locations/0/country: "Germany"
    /locations/0/city: "Berlin"
    /locations/1/country: "France"
    /locations/1/city: "Paris"
    /headquarters/country: "Belgium"
    /headquarters/employees: 250
    /exports/0/city: "Moscow"
    /exports/1/city: "Athens"

Bir öğe yazıldığında, Azure Cosmos DB her özelliğin yolu ve karşılık gelen değeri etkili bir şekilde dizinler.

## <a name="index-kinds"></a>Dizin türleri

Azure Cosmos DB, şu anda iki tür dizinleri destekler:

**Aralığı** dizin türü için kullanılır:

- Eşitlik sorguları için: `SELECT * FROM container c WHERE c.property = 'value'`
- Aralık sorguları: `SELECT * FROM container c WHERE c.property > 'value'` (çalışır `>`, `<`, `>=`, `<=`, `!=`)
- `ORDER BY` sorgular: `SELECT * FROM container c ORDER BY c.property`
- `JOIN` sorgular: `SELECT child FROM container c JOIN child IN c.properties WHERE child = 'value'`

Aralık dizinleri skaler değerler (dize veya sayı) kullanılabilir.

**Uzamsal** dizin türü için kullanılır:

- Jeo-uzamsal uzaklık sorgular: `SELECT * FROM container c WHERE ST_DISTANCE(c.property, { "type": "Point", "coordinates": [0.0, 10.0] }) < 40`
- Jeo-uzamsal sorguları içinde: `SELECT * FROM container c WHERE ST_WITHIN(c.property, {"type": "Point", "coordinates": [0.0, 10.0] } })`

Uzaysal dizinler kullanılabilir üzerinde düzgün biçimlendirilmiş [GeoJSON](geospatial.md) nesneleri. Noktaları, LineStrings ve çokgenler desteklenmemektedir.

## <a name="querying-with-indexes"></a>Dizinler ile sorgulama

Verileri sıralarken ayıklanan yolları sorgu işlenirken dizinini aramak kolaylaştırır. Eşleşen tarafından `WHERE` dizinli yollarının listesini ile yan tümcesi bir sorgu mümkündür çok hızlı bir şekilde sorgu koşulu karşılayan öğeleri tanımlamak.

Örneğin, şu sorguyu inceleyin: `SELECT location FROM location IN company.locations WHERE location.country = 'France'`. (Herhangi bir yerde "Fransa" kendi ülke sahip olduğu öğeler üzerinde filtreleme) sorgu koşulu aşağıdaki kırmızı renkte vurgulanmış yolu eşleşir:

![Bir ağaç içindeki belirli bir yol ile eşleşen](./media/index-overview/matching-path.png)

> [!NOTE]
> Bir `ORDER BY` yan tümcesi *her zaman* aralığı gereken dizin ve başvurduğu yolu bir sahip değilse başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler de dizin oluşturma hakkında daha fazla bilgi edinin:

- [Dizin oluşturma ilkesi](index-policy.md)
- [Dizin oluşturma ilkesini yönetme](how-to-manage-indexing-policy.md)
