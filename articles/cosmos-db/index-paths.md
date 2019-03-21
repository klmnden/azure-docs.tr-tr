---
title: Azure Cosmos DB'de dizin yolları ile çalışma
description: Azure Cosmos DB'de dizin yolları genel bakış
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 3/13/2019
ms.author: mjbrown
ms.openlocfilehash: d0fce763822ded374eab2f70c3f319aba0c89267
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57992829"
---
# <a name="index-paths-in-azure-cosmos-db"></a>Azure Cosmos DB'de dizin yolları

Azure Cosmos DB'de dizin yolları kullanarak, dahil etmek veya belirli bir yol dizine elmadan hariç tutmak seçebilirsiniz. Belirli yollar seçme, geliştirilmiş yazma performansını ve sorgu desenleri bildiğiniz senaryoları için daha düşük dizin depolaması sunar. Dizin kök yolları başlama (`/`) joker karakter işleci ve genellikle bitemez `?` joker karakter işleci. Bu düzen, birden çok olası değerler önek olduğunu gösterir. Örneğin, sorgu hizmet `SELECT * FROM Families F WHERE F.familyName = "Andersen"`, bir dizin yolunu içermelidir `/familyName/?` kapsayıcının dizin ilkesi.

Dizin yolları da kullanabilirsiniz `*` yolları yinelemeli olarak ön ek altındaki davranışını belirtmek için joker karakter işleci. Örneğin, `/payload/*` yükü özelliği altında her şeyi dizine elmadan hariç tutmak için kullanılabilir.

## <a name="common-patterns-for-index-paths"></a>Dizin yolları için ortak desenler

Dizin yolları belirtmek için ortak desenler şunlardır:

| **Path** | **Açıklama/kullanım örneği** |
| ---------- | ------- |
| /          | Koleksiyon için varsayılan yolu. Özyinelemeli ve tüm belgeyi ağaca uygular.|
| / prop /?    | Dizin yolu sorguları aşağıdaki gibi gerekli (aralık türleriyle sırasıyla): <br><br>SELECT FROM koleksiyon c WHERE c.prop = "değer"<br><br>SELECT FROM koleksiyon c WHERE c.prop > 5 <br><br>Koleksiyon c ORDER BY c.prop seçin  |
| / prop / *    | Belirtilen etiket altında tüm yolları için dizin yolu. Aşağıdaki sorgular ile çalışır <br><br>SELECT FROM koleksiyon c WHERE c.prop = "değer"<br><br>SELECT FROM koleksiyon c WHERE c.prop.subprop > 5<br><br>SELECT FROM koleksiyon c WHERE c.prop.subprop.nextprop = "değer"<br><br>Koleksiyon c ORDER BY c.prop seçin |
| / Özellikler / [] /?| Dizin yolu, yineleme sunmak ve skalerler ["a", "b", "c"] gibi bir dizi sorguları katılmak için gerekli:<br><br>WHERE etiketi seçin etiketi etiketi IN collection.props = "değer"<br><br>Koleksiyon c birleşim etiketi IN c.props etiketini seçin > 5 burada etiketi |
| [] /subprop/ /props/? | Dizin yolu gerekli yineleme yapacak ve nesne dizileri sorguları birleştirme gibi [{subprop: "a"}, {subprop: "b"}]:<br><br>WHERE tag.subprop seçin etiketi etiketi IN collection.props = "değer"<br><br>WHERE tag.subprop seçin etiketi koleksiyon c birleşim etiketi IN c.props = "değer" |
| / prop/subprop /? | Dizin yolu sorguları gerekli (aralık türleriyle sırasıyla):<br><br>SELECT FROM koleksiyon c WHERE c.prop.subprop = "değer"<br><br>SELECT FROM koleksiyon c WHERE c.prop.subprop > 5  |

Özel dizin yolları ayarladığınızda, özel bir yol gösterilen öğenin tamamı için varsayılan dizin oluşturma kuralı belirtmek için gerekmesinden `/*`.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde Azure Cosmos DB'de dizinleme hakkında daha fazla bilgi edinin:

- [Dizin oluşturma genel bakış](index-overview.md)
- [Azure Cosmos DB'de dizinleme ilkeleri](indexing-policies.md)
- [Azure Cosmos DB'de dizin türleri](index-types.md)
