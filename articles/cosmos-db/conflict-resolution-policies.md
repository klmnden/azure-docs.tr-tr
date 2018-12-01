---
title: Çakışma çözüm türlerini ve Azure Cosmos DB'de çözümleme ilkeleri
description: Bu makalede, çakışma kategorileri ve Azure Cosmos DB'de Çakışma çözümlemesi ilkeleri açıklanır.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 5506b27ce56ca7a83ce16aab818767a392d77430
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52680303"
---
# <a name="conflict-types-and-resolution-policies"></a>Çakışma türleri ve çözme ilkeleri

Çakışma ve çakışma çözümü ilkeleri Azure Cosmos DB hesabınızın birden çok ile yapılandırılmışsa, geçerli olan bölgeler yazın.

Yazarları aynı anda birden çok bölgede aynı öğesi güncelleştirdiğinizde birden çok yazma bölgeleri ile yapılandırılmış Azure Cosmos DB hesapları için güncelleştirme çakışmaları oluşabilir. Güncelleştirme çakışmaları aşağıdaki üç tür olarak sınıflandırılan:

* **Çakışmaları Ekle**: uygulama, iki veya daha fazla öğe aynı benzersiz dizini ile aynı anda iki veya daha fazla bölgelerden ekler. bu çakışmaları oluşabilir. Örneğin, bir ID özelliği ile bu çakışma ortaya çıkabilir. Tüm yazma işlemlerini başlangıçta kendi ilgili yerel bölgelerde başarılı olabilir. Ancak, yalnızca bir öğe özgün kimliği ile seçtiğiniz çakışma çözüm İlkesi bağlı olarak, son olarak taahhüt eder.

* **Çakışmaları değiştirin**: bir uygulamayı aynı anda bölgelerden iki veya daha fazla tek bir öğe güncelleştirildiğinde bu çakışmaları oluşabilir.

* **Çakışmaları Sil**: bir uygulamayı aynı anda bir bölgeden bir öğeyi siler ve başka bir bölgede güncelleştirir, bu çakışmaları oluşabilir.

## <a name="conflict-resolution-policies"></a>Çakışma çözümleme ilkeleri

Azure Cosmos DB güncelleştirme çakışmaları çözümlemek için esnek bir ilke odaklı mekanizması sağlar. Bir Azure Cosmos DB kapsayıcısında iki çakışma çözüm İlkesi arasından seçim yapabilirsiniz:

- **Son yazma WINS (LWW)**: bir sistem tanımlı bir zaman damgası özelliği varsayılan olarak, bu çözümleme İlkesi kullanır. Bu zaman eşitleme saati protokolünü temel alır. Azure Cosmos DB SQL API'SİNİN kullanıyorsanız, Çakışma çözümlemesi için kullanılan diğer özelliklerden herhangi birini özel sayısal belirtebilirsiniz. Özel bir sayısal özellik çakışma çözümleme yolu adlandırılır. 

  İki veya daha fazla öğe çakışma Ekle veya Değiştir işlemleri çakışma çözümleme yolu için en yüksek değer olan öğenin birinci dönüşür. Sistem, aynı sayısal değere çakışma çözümleme yolu için birden çok öğe varsa, birinci belirler. Tüm bölgeler ayarlamak için bir tek kazanan birinciyi ve son taahhüt öğenin aynı sürümü ile yakınsanmasını garanti edilir. Çakışmaları dahilse, Sil silinen sürümü her zaman WINS zaman ekleyemez veya çakışmaları değiştirin. Bu sonucu çakışma çözümleme yolu değeri ne olursa olsun gerçekleşir.

  > [!NOTE]
  > Son yazma WINS varsayılan çakışma çözümü ilkesi var. SQL, Azure Cosmos DB tablo, MongoDB, Cassandra ve Gremlin API hesapları için kullanılabilir.

  Daha fazla bilgi için bkz. [LWW kullanan örnekler Çakışma çözümlemesi ilkelerinin](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

- **Özel**: Bu çözümü İlkesi uygulama tanımlı semantiği çakışmaları karşılaştırmak için tasarlanmıştır. Bu ilke Azure Cosmos DB kapsayıcınız ayarladığınızda, ayrıca bir birleştirme depolanan yordam kaydetmeniz gerekir. Bu yordamı, veritabanı işlem sunucusunda altında çakışmaları algılandığında otomatik olarak çağrılır. Tam olarak sistemidir taahhüt protokolünün bir parçası olarak bir birleştirme yordamının yürütülmesi için bir kez garanti.  

  Kapsayıcınızın özel bir çözüm seçeneğiyle yapılandırırsanız, dikkat edilmesi gereken iki noktalar vardır. Bir birleştirme yordam kapsayıcı üzerindeki kaydetmek başarısız veya birleştirme yordamı, çalışma zamanında bir özel durum oluşturur, çakışmaları akışı çakışmaları yazılır. Uygulamanızı daha sonra el ile besleme çakışıyor çakışmaları gerekir. Daha fazla bilgi için bkz. [özel çözümleme İlkesi kullanmayı ve akış çakışmaları kullanma örnekleri](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy).

  > [!NOTE]
  > Özel çakışma çözüm ilkesi yalnızca SQL API'si hesapları için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Çakışma çözümlemesi ilkelerinin nasıl yapılandırılacağını öğrenin. Aşağıdaki makalelere bakın:

* [LWW çakışma çözümü İlkesi kullanın](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Özel çakışma çözümü İlkesi kullanın](how-to-manage-conflicts.md#create-a-last-writer-wins-conflict-resolution-policy)
* [Akış çakışmaları kullanın](how-to-manage-conflicts.md#read-from-conflict-feed)
