---
title: Azure Cosmos DB'de çakışma çözümü
description: Bu makalede, çakışma kategorileri ve Azure Cosmos DB'de Çakışma çözümlemesi ilkeleri açıklanır.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 2f219ed6c3155e8b7d3d978116e1748f6c115fea
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244118"
---
# <a name="conflict-types-and-resolution-policies"></a>Çakışma türlerini ve çözümleme ilkeleri

Çakışma ve çakışma çözümü ilkeleri Cosmos hesabınızın birden çok ile yapılandırılmışsa, geçerli olan bölgeler yazın.

Birden çok yazan eşzamanlı olarak birden çok bölgede aynı öğesi güncelleştirdiğinizde birden çok yazma bölgeleri ile yapılandırılmış, Cosmos DB hesapları için güncelleştirme çakışmaları oluşabilir. Güncelleştirme çakışmaları aşağıdaki üç tür olarak sınıflandırılan:

1. **Çakışmaları Ekle** bir uygulama, iki veya daha fazla öğe ile aynı benzersiz dizin (örneğin, ID özelliği) aynı anda iki veya daha fazla bölgelerden ekler. oluşabilir. Bu durumda, tüm yazma işlemlerini başlangıçta kendi ilgili yerel bölgelerde başarılı olabilir, ancak yalnızca bir öğe özgün kimliği ile seçtiğiniz çakışma çözüm İlkesi bağlı olarak, son olarak taahhüt eder.
2. **Çakışmaları değiştirin** bir uygulamayı tek bir öğesi iki veya daha fazla bölgede aynı anda güncelleştiren oluşabilir.
3. **Çakışmaları Sil** uygulamanın aynı anda bir bölgeden bir öğeyi siler ve başka bir bölgeden güncelleştirmeleri oluşabilir.

## <a name="conflict-resolution-policies"></a>Çakışma çözümleme ilkeleri

Cosmos DB güncelleştirme çakışmaları çözümlemek için esnek bir ilke odaklı mekanizması sunar. Cosmos kapsayıcısı üzerinde aşağıdaki iki Çakışma çözümlemesi ilkeler arasından seçim yapabilirsiniz:

- **Son yazma WINS (LWW)** , varsayılan olarak kullanır (zaman eşitleme saati protokolü temel alarak) sistem tanımlı bir zaman damgası özellik. Alternatif olarak, SQL API'si kullanılırken, Cosmos DB ("çakışma çözümleme yolu" da bilinir) diğer özelliklerden herhangi birini özel sayısal Çakışma çözümlemesi için kullanılacak belirtmenizi sağlar.  

İki veya daha fazla öğe çakışma Ekle veya Değiştir işlemleri, "çakışma çözümleme yolu" için en yüksek değeri içeren bir öğe "Birinci" olur. Çakışma çözümleme yolu için aynı sayısal değere birden çok öğe varsa, seçili "Birincisi" sürümü sistem tarafından belirlenir. Tüm bölgeler ayarlamak için bir tek kazanan birinciyi ve son taahhüt öğenin aynı sürümü ile yakınsanmasını garanti edilir. Varsa ilgili çakışmalar Sil silinen sürümden her zaman ekler veya çakışma çözümleme yolu değerinden bağımsız olarak değiştir çakışmaları üzerinden kazanır.

> [!NOTE]
> Varsayılan çakışma çözümü İlkesi LWW olduğundan ve SQL, tablo, MongoDB, Cassandra ve Gremlin API'leri için kullanılabilir.

Daha fazla bilgi için bkz. [LWW kullanan örnekler Çakışma çözümlemesi ilkelerinin](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

- **Özel** Bu ilke, uygulama tanımlı semantiği çakışmaları karşılaştırmak için tasarlanmıştır. Cosmos kapsayıcınızın Bu ilkeyi düzenlerken, ayrıca sunucu tarafında veritabanı işlem altında güncelleştirme çakışmaları algılandığında otomatik olarak çağrılır bir birleştirme depolanan yordam, kaydetmeniz gerekir. Tam olarak sistemidir taahhüt protokolünün bir parçası olarak bir birleştirme yordamının yürütülmesi için bir kez garanti.  

Ancak, özel bir çözüm seçeneğiyle kapsayıcınızı yapılandırın, ancak herhangi bir birleştirme yordam kapsayıcı üzerindeki kaydı başarısız veya birleştirme yordamı çalışma zamanında bir özel durum oluşturursa, çakışmaları akışı çakışmaları yazılır. Uygulamanızı çakışmaları akışta çakışmaları el ile çözümlemeniz gerekir. Daha fazla bilgi için bkz. [özel çözümleme İlkesi ve nasıl akış çakışmaları kullanılacağını kullanma örnekleri](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

> [!NOTE]
> Özel çakışma çözüm ilkesi yalnızca SQL API'si hesapları için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki çakışma çözümleme ilkelerini aşağıdaki makaleleri kullanarak yapılandırma hakkında bilgi edinebilirsiniz:

* [LWW çakışma çözümü İlkesi kullanma](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Özel çakışma çözümü İlkesi kullanma](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Çakışmaları akışı kullanma](how-to-manage-conflicts.md#read-from-conflict-feed)
