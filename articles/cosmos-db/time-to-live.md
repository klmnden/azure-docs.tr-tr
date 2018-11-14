---
title: Yaşam süresi ile Azure Cosmos DB'de verileri süresi dolacak
description: TTL ile Microsoft Azure Cosmos DB sistemden bir süre sonra otomatik olarak temizlenir belgeleriniz olanağı sağlar.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: mjbrown
ms.openlocfilehash: c08c171e3a95b0d0f408660a7ec9021ca0323fbd
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621285"
---
# <a name="time-to-live-for-azure-cosmos-db-data"></a>Azure Cosmos DB verilerin yaşam süresi

"Time to Live" veya TTL, Azure Cosmos DB öğeleri belirli bir zaman aralığına sonra otomatik olarak bir kapsayıcıdan silme olanağı sağlar. Varsayılan olarak, zaman kapsayıcı düzeyinde canlı ve geçersiz kılma değeri öğe başına temelinde ayarlayabilirsiniz. Bir kapsayıcı veya bir öğe düzeyinde TTL ayarladıktan sonra Azure Cosmos DB son değiştirme zamanı beri süre sonra otomatik olarak bu öğeleri kaldırır. Saniye cinsinden yaşam süresi değeri yapılandırılır. TTL yapılandırdığınızda, sistem otomatik olarak açıkça istemci uygulaması tarafından verilen bir silme işlemi tersine TTL değere göre süresi dolan öğeleri silin.

## <a name="time-to-live-for-containers-and-items"></a>Kapsayıcılar ve öğeler için yaşam süresi

Saniye cinsinden yaşam süresi değeri ayarlanır ve bir öğenin son değiştirilme zamanından bir değişim olarak yorumlanır. Bir kapsayıcı veya bir öğe kapsayıcı içindeki yaşam süresi ayarlayabilirsiniz:

1. **Bir kapsayıcıda yaşam süresi** (kullanılarak ayarlanan `DefaultTimeToLive`):

   - Eksikse (veya null olarak ayarlanır) öğeleri olmayan süresi dolmuş otomatik olarak.

   - Mevcut ve değeri ayarlandığında, "-1" sonsuz olarak eşit olup – öğeleri varsayılan olarak dolmasın.

   - Öğeleri var ve değeri ayarlandığında, bazı numarasına ("n") – "n" saniye, son değiştirme zamanı sonra süresi dolar.

2. **Yaşam süresi bir öğe üzerinde** (kullanılarak ayarlanan `TimeToLive`):

   - Bu özellik geçerli yalnızca `DefaultTimeToLive` mevcut olduğundan ve ayarlı değil için üst kapsayıcı null.

   - Varsa, onu geçersiz kılar `DefaultTimeToLive` üst kapsayıcı değeri.

## <a name="time-to-live-configurations"></a>Canlı yapılandırmaları süresi

* Bir kapsayıcı TTL 'n' olarak ayarlanırsa, bu kapsayıcı öğeler n saniye sonra sona erecek.  Canlı için -1 (süresinin sona ermediğinden gösteren) olarak ayarlayın, kendi zamanınız öğe ile aynı kapsayıcıda veya bazı öğeler, farklı bir sayıyla ayar yaşam süresini kıldıysanız, bu öğeler süreleri sona ererse yapılandırılmış TTL değerine göre. 

* TTL bir kapsayıcı ayarlanmazsa, bu kapsayıcı içinde bir öğe yaşam süresini etkiye sahip değildir. 

* Bir kapsayıcı TTL -1 olarak ayarlanırsa, zaman n, Canlı kümesine sahiptir, bu kapsayıcıdaki bir öğe n saniye sonra sona erecek ve kalan öğeleri dolmaz. 

TTL temel öğeleri silmek ücretsizdir. Hiçbir ek ücret yoktur (diğer bir deyişle, hiçbir ek RU tüketilir) sonucunda TTL bitiş öğesi silindiğinde.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde Canlı kutucuğu yapılandırmayı öğrenin:

* [Yaşam süresi yapılandırma](how-to-time-to-live.md)
