---
title: Çakışma çözüm türlerini ve çözümleme ilkeleri birden çok bölgede Azure Cosmos DB'de yazma
description: Bu makalede, çakışma kategorileri ve Azure Cosmos DB'de Çakışma çözümlemesi ilkeleri açıklanır.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 98e9f5fff1b74d417ee07ed0056c8046b49baa17
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236541"
---
# <a name="conflict-types-and-resolution-policies"></a>Çakışma türleri ve çözme ilkeleri

Çakışma ve çakışma çözümü ilkeleri Azure Cosmos DB hesabınızın birden çok ile yapılandırılmışsa, geçerli olan bölgeler yazın.

Yazarları aynı anda birden çok bölgede aynı öğesi güncelleştirdiğinizde birden çok yazma bölgeleri ile yapılandırılmış Azure Cosmos hesapları için güncelleştirme çakışmaları oluşabilir. Güncelleştirme çakışmaları aşağıdaki üç türde olabilir:

* **Çakışmaları Ekle**: Bir uygulama, iki veya daha fazla öğe aynı benzersiz dizini ile aynı anda iki veya daha fazla bölgede ekler. bu çakışmaları oluşabilir. Örneğin, bir ID özelliği ile bu çakışma ortaya çıkabilir.

* **Çakışmaları değiştirin**: Bir uygulamayı aynı anda iki veya daha fazla bölgede aynı öğede güncelleştirdiğinde bu çakışmaları oluşabilir.

* **Çakışmaları Sil**: Bir uygulamayı aynı anda tek bir bölge içindeki bir öğeyi siler ve başka bir bölgede güncelleştirir, bu çakışmaları oluşabilir.

## <a name="conflict-resolution-policies"></a>Çakışma çözümleme ilkeleri

Azure Cosmos DB yazma çakışmaları çözümlemek için esnek bir ilke odaklı mekanizması sağlar. Bir Azure Cosmos kapsayıcısı üzerinde iki çakışma çözüm İlkesi arasından seçim yapabilirsiniz:

- **WINS (LWW)'son yazma**: Bu çözüm ilke varsayılan olarak, bir sistem tanımlı bir zaman damgası özelliği kullanır. Bu zaman eşitleme saati protokolünü temel alır. SQL API'si kullanırsanız, Çakışma çözümlemesi için kullanılan diğer özelliklerden herhangi birini özel sayısal (örneğin, bir zaman damgası kendi kavramı) belirtebilirsiniz. Özel bir sayısal özellik, ayrıca olarak adlandırılır *çakışma çözümleme yolu*. 

  İki veya daha fazla öğe çakışma Ekle veya Değiştir işlemleri çakışma çözümleme yolu için en yüksek değer olan öğenin birinci dönüşür. Sistem, aynı sayısal değere çakışma çözümleme yolu için birden çok öğe varsa, birinci belirler. Tüm bölgeler ayarlamak için bir tek kazanan birinciyi ve son taahhüt öğenin aynı sürümü ile yakınsanmasını garanti edilir. Çakışmaları dahilse, Sil silinen sürümü her zaman WINS zaman ekleyemez veya çakışmaları değiştirin. Çakışma çözümleme yolu değeri ne olursa olsun, bu sonucu oluşur.

  > [!NOTE]
  > Son yazma WINS varsayılan çakışma çözümü ilkesi var. Aşağıdaki API'leri için kullanılabilir: SQL, MongoDB, Cassandra, Gremlin ve tablo.

  Daha fazla bilgi için bkz. [LWW kullanan örnekler Çakışma çözümlemesi ilkelerinin](how-to-manage-conflicts.md).

- **Özel**: Bu çözüm İlkesi uygulama tanımlı semantiği çakışmaları karşılaştırmak için tasarlanmıştır. Bu ilke Azure Cosmos kapsayıcınızın ayarladığınızda, ayrıca kayıt olmanız gereklidir bir *saklı yordam birleştirme*. Bu yordam, çakışma, sunucu üzerindeki veritabanı işlem altında algılandığında otomatik olarak çağrılır. Tam olarak sistemidir taahhüt protokolünün bir parçası olarak bir birleştirme yordamının yürütülmesi için bir kez garanti.  

  Kapsayıcınızın özel bir çözüm seçeneğiyle yapılandırmak ve kaydetmek kapsayıcı üzerindeki bir birleştirme yordam başarısız veya birleştirme yordamı, çalışma zamanında bir özel durum oluşturur, çakışmaları yazılan *akışı çakışmaları*. Uygulamanızı daha sonra el ile besleme çakışıyor çakışmaları gerekir. Daha fazla bilgi için bkz. [özel çözümleme İlkesi kullanmayı ve akış çakışmaları kullanma örnekleri](how-to-manage-conflicts.md).

  > [!NOTE]
  > Özel çakışma çözüm ilkesi yalnızca SQL API'si hesapları için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Çakışma çözümlemesi ilkelerinin nasıl yapılandırılacağını öğrenin:

* [Çok yöneticili uygulamalarınızda yapılandırma](how-to-multi-master.md)
* [Çakışma çözümleme ilkelerini yönetme](how-to-manage-conflicts.md)
* [Akış çakışmalarından okuma](how-to-manage-conflicts.md#read-from-conflict-feed)
