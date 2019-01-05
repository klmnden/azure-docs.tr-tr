---
title: Azure Cosmos DB'de dizin türleri
description: Azure Cosmos DB'de dizin türlerine genel bakış
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/5/2018
ms.author: rimman
ms.openlocfilehash: 44fe262dc28a016af9eb01f28278b2c3d81d9034
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54034099"
---
# <a name="index-types-in-azure-cosmos-db"></a>Azure Cosmos DB'de dizin türleri

Yolu için dizin oluşturma ilkesini yapılandırdığınız birden çok seçenek vardır. Her yol için bir veya daha fazla dizin tanımları belirtebilirsiniz:

- **Veri türü:** Dize, sayı, nokta, çokgen veya LineString (yol başına veri türü başına yalnızca bir girdi içerebilir).

- **Dizin türü:** Karma (eşitlik sorguları), aralığı (eşitlik, aralığı veya ORDER BY sorguları) veya uzamsal (uzamsal sorgular).

- **Duyarlık:** Bir karma dizine bu 1 ile 8 hem dize hem de sayılarla için farklılık gösterir ve varsayılan değer 3'tür. Bir aralık dizin için en yüksek duyarlık değeri -1'dir. 1 ile 100 (en yüksek duyarlık) dize veya sayı değerleri arasında değişebilir.

## <a name="index-kind"></a>Dizin türü

Azure Cosmos DB, dize veya sayı veri türleri için yapılandırılmış her bir yol veya her ikisi için karma dizine ve aralık dizini destekler.

- **Karma dizine** verimli eşitlik ve JOIN sorgularını destekler. Kullanım örnekleri için varsayılan değer 3 bayt daha yüksek bir duyarlık karma dizinler gerekmez. Veri türü dize veya sayı olabilir.

- **Aralık dizini** verimli eşitlik sorguları, aralık sorguları destekler (kullanarak >, <>, =, < =,! =) ve ORDER BY sorgular. ORDER By sorguları varsayılan olarak, ayrıca maksimum dizin duyarlık (-1) gerektirir. Veri türü dize veya sayı olabilir.

- **Uzamsal dizin** destekler verimli uzamsal (içinde ve uzaklık) sorgular. Veri türü, nokta, çokgen veya LineString olabilir. Azure Cosmos DB, nokta, çokgen veya LineString veri türleri için belirtilebilir her yol için uzamsal dizin türü de destekler. Değer belirtilen yolda gibi geçerli bir GeoJSON parçası olmalıdır {"type": "Nokta", "koordinatları": [0.0, 10.0]}. Azure Cosmos DB, otomatik dizin oluşturma noktası Çokgen ve LineString veri türlerini destekler.

Uzaysal dizinler, hizmet için kullanılabilir ve karma, sorgu aralığı örnekleri aşağıda verilmiştir:

| **Dizin türü** | **Açıklama/kullanım örneği** |
| ---------- | ---------------- |
| Karma  | / Prop / karma? (veya /) aşağıdaki sorguları verimli bir şekilde sunmak için kullanılabilir:<br><br>SELECT FROM koleksiyon c WHERE c.prop = "değer"<br><br>Karma üzerinden/özellikler / [] /? (veya / veya/Özellikler /) aşağıdaki sorguları verimli bir şekilde sunmak için kullanılabilir:<br><br>WHERE etiketi seçin etiketi koleksiyon c birleşim etiketi IN c.props = 5  |
| Aralık  | / Prop / aralığı? (veya /) aşağıdaki sorguları verimli bir şekilde sunmak için kullanılabilir:<br><br>SELECT FROM koleksiyon c WHERE c.prop = "değer"<br><br>SELECT FROM koleksiyon c WHERE c.prop > 5<br><br>Koleksiyon c ORDER BY c.prop seçin   |
| Uzamsal     | / Prop / aralığı? (veya /) aşağıdaki sorguları verimli bir şekilde sunmak için kullanılabilir:<br><br>SELECT FROM c koleksiyonu<br><br>WHERE ST_DISTANCE (c.prop, {"type": "Nokta", "koordinatları": [0.0, 10.0]}) < 40<br><br>Burada ST_WITHIN(c.prop, {"type": Koleksiyon C'den seçin "Çokgen",...}) --Etkin noktalarında dizin ile<br><br>Burada ST_WITHIN({"type": Koleksiyon C'den seçin "Nokta",...}, c.prop)--üzerinde çokgenler etkin dizin ile.     |

## <a name="default-behavior-of-index-kinds"></a>Dizin türleri varsayılan davranışı

- Sinyal bir tarama sorgu böyle bir durumda, varsayılan olarak, hizmet vermek gerekli olabilir herhangi kesinlik aralığı dizin yok ise sorgularla işleçler gibi aralığı için bir hata döndürülür > =.

- "X-ms-documentdb-enable-tarama" üst bilgisinde REST API veya "EnableScanInQuery" istek seçeneği .NET SDK'sı kullanarak olmadan bir aralık dizini aralık sorguları gerçekleştirilebilir. Azure Cosmos DB dizine göre filtrelemek için kullanabileceğiniz sorgusunda herhangi bir filtre varsa, hata döndürülür.

- Uzamsal dizin veya dizinden sunulabilen diğer filtrelerle değilse, varsayılan olarak, uzamsal sorgular için bir hata döndürülür. Bir tarama sorgularını x-ms-documentdb-enable-tarama veya EnableScanInQuery kullanarak gerçekleştirilebilir.

## <a name="index-precision"></a>Dizin duyarlık

- Dizin duyarlık dizin depolama yükü ve sorgu performansı arasında bir denge sağlamak için kullanabilirsiniz. Sayılar için varsayılan duyarlık yapılandırma-1 (maksimum) kullanmanızı öneririz. Sayı 8 baytlık JSON olduğundan, bu 8 baytlık bir yapılandırmaya eşdeğerdir. Bazı aralıklar dahilinde değerleri aynı eşleyin anlamına gelir, duyarlığı, 1 ile 7 arasında gibi daha düşük bir değer seçme giriş dizini. Bu nedenle, dizin depolama alanı azaltabilir, ancak sorgu yürütme daha fazla öğe işlemek zorunda kalabilirsiniz. Sonuç olarak, daha fazla aktarım hızı/RU tüketir.

- Dizin duyarlık dize aralıklarıyla daha pratik uygulama vardır. Dizeleri herhangi bir rastgele uzunluktaki olabileceğinden, dizin duyarlık seçimi dize aralığı sorguların performansını etkileyebilir. Ayrıca, gerekli olan dizin depolama alanı miktarı da etkileyebilir. Dizinleri dizesi aralık 1 ile 100 veya -1 (maksimum) arasında bir dizin duyarlıkla yapılandırılabilir. Dize özellikleri ORDER BY sorguları gerçekleştirmek istiyorsanız, bir duyarlık karşılık gelen yollarla için-1 belirtmeniz gerekir.

- Uzaysal dizinler, her zaman varsayılan dizini duyarlık tüm türleri için (Point, LineString ve Çokgen) kullanın. Uzaysal dizinler için varsayılan dizin duyarlık geçersiz kılınamaz.

Bir sorgu ORDER BY kullanır ancak karşı en yüksek duyarlık sorgulanan yoluyla bir aralık dizini yok, azure Cosmos DB, bir hata döndürür.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de dizinleme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Dizin oluşturma genel bakış](index-overview.md)
- [Dizin oluşturma ilkesi](indexing-policies.md)
- [Dizin yolları](index-paths.md)

