---
title: Yaşam süresi ile Azure Cosmos DB'de verileri süresi dolacak
description: TTL ile Microsoft Azure Cosmos DB sistemden bir süre sonra otomatik olarak temizlenir belgeleriniz olanağı sağlar.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 27540c3dfce73788e01f0f8ab0892c733f153fdf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60729010"
---
# <a name="time-to-live-ttl-in-azure-cosmos-db"></a>Azure Cosmos DB süresi (TTL) 

İle **yaşam süresi** veya TTL, Azure Cosmos DB öğeleri belirli bir zaman aralığına sonra otomatik olarak bir kapsayıcıdan silme olanağı sağlar. Varsayılan olarak, zaman kapsayıcı düzeyinde canlı ve geçersiz kılma değeri öğe başına temelinde ayarlayabilirsiniz. Bir kapsayıcı veya bir öğe düzeyinde TTL ayarladıktan sonra Azure Cosmos DB son değiştirme zamanı beri süre sonra otomatik olarak bu öğeleri kaldırır. Saniye cinsinden yaşam süresi değeri yapılandırılır. TTL yapılandırdığınızda, sistem otomatik olarak TTL değeri, istemci uygulama tarafından açıkça verildiği bir silme işlemi gerek kalmadan temelinde süresi dolan öğeleri silin.

## <a name="time-to-live-for-containers-and-items"></a>Kapsayıcılar ve öğeler için yaşam süresi

Saniye cinsinden yaşam süresi değeri ayarlanır ve bir öğenin son değiştirilme zamanından bir değişim olarak yorumlanır. Bir kapsayıcı veya bir öğe kapsayıcı içindeki yaşam süresi ayarlayabilirsiniz:

1. **Bir kapsayıcıda yaşam süresi** (kullanılarak ayarlanan `DefaultTimeToLive`):

   - Eksikse (veya null olarak ayarlanır) öğeleri olmayan süresi dolmuş otomatik olarak.

   - Mevcut ve değeri ayarlandığında, "-1" sonsuza eşittir ve öğeleri varsayılan olarak dolmasın.

   - Mevcut ve değeri ayarlandığında, bazı sayıya *"n"* – öğeleri sona erecek *"n"* sonra son değiştirme zamanı saniye.

2. **Yaşam süresi bir öğe üzerinde** (kullanılarak ayarlanan `ttl`):

   - Bu özellik geçerli yalnızca `DefaultTimeToLive` mevcut olduğundan ve ayarlı değil için üst kapsayıcı null.

   - Varsa, onu geçersiz kılar `DefaultTimeToLive` üst kapsayıcı değeri.

## <a name="time-to-live-configurations"></a>Canlı yapılandırmaları süresi

* TTL ayarlanırsa *"n"* bir kapsayıcı, sonra öğeleri bu kapsayıcı içinde dolacağını *n* saniye.  Canlı için -1 (süresinin sona ermediğinden gösteren) olarak ayarlayın, kendi zamanınız öğe ile aynı kapsayıcıda veya bazı öğeler, farklı bir sayıyla ayar yaşam süresini kıldıysanız, bu öğelerin süresinin üzerinde yapılandırılmış bir TTL değeri kendi temel. 

* TTL bir kapsayıcı ayarlanmazsa, bu kapsayıcı içinde bir öğe yaşam süresini etkiye sahip değildir. 

* Bir kapsayıcı TTL -1 olarak ayarlanırsa, zaman n, Canlı kümesine sahiptir, bu kapsayıcıdaki bir öğe n saniye sonra sona erecek ve kalan öğeleri dolmaz. 

TTL temel öğeleri silmek ücretsizdir. Hiçbir ek ücret yoktur (diğer bir deyişle, hiçbir ek RU tüketilir) sonucunda TTL bitiş öğesi silindiğinde.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler de yaşam süresi yapılandırma işlemleri gerçekleştirmeyi öğreneceksiniz:

* [Yaşam süresi yapılandırma](how-to-time-to-live.md)
