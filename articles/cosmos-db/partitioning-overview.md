---
title: Azure Cosmos DB bölümleme
description: Azure Cosmos DB'de bölümleme genel bakış
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: andrl
ms.openlocfilehash: b89830d566b36b0446836d8f32aee5756e2d0991
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52498422"
---
# <a name="partitioning-in-azure-cosmos-db"></a>Azure Cosmos DB bölümleme

Bölümleme, uygulamanızın performans gereksinimlerini karşılamak için bir veritabanında tek tek kapsayıcı ölçeklendirme işleminden Cosmos DB tarafından kullanılan tekniğidir. Bölümlemeyi kullanarak, bir kapsayıcı öğeleri mantıksal bölümler adlı ayrı altkümelere, ayrılır. Mantıksal bölümleri her öğeyle ilişkili bir bölüm anahtarı özelliğinin değeri temel alınarak oluşturulur.

Bir mantıksal bölüm, bir kapsayıcıdaki öğelerin ayrı bir alt kümesidir. Öğeleri mantıksal bir bölümdeki tüm öğeleri mantıksal bölümün paylaşılan bölüm anahtarı değeri tarafından tanımlanır.  Örneğin, belgeleri tutan kapsayıcı göz önünde bulundurun ve her belge sahip bir `UserID` özelliği.  Varsa `UserID` kapsayıcı öğeleri için bölüm gören anahtar ve 1000 benzersiz vardır `UserID` değerleri, 1000 mantıksal bölümler için kapsayıcı oluşturulur.

Bir kapsayıcıdaki her bir öğesi olan bir **bölüm anahtarı** öğenin belirleyen **mantıksal bölüm**, ve her bir öğe de sahip bir **öğesi kimliği** (olduğu mantıksal içinde benzersiz Bölüm).  **Dizin** , öğeyi benzersiz olarak tanımlayan ve bölüm anahtarı ve öğe kimliği birleştirerek oluşturulur.

Bir bölüm anahtarı seçmeyi uygulamanızın performansını etkileyen önemli bir karardır.  Daha fazla bilgi için bkz. [bir bölüm anahtarı seçmeyi](partitioning-overview.md#choose-partitionkey) makalede ayrıntılı yönergeler için.

## <a name="logical-partition-management"></a>Mantıksal bölüm yönetimi

Cosmos DB, şeffaf ve otomatik olarak mantıksal bölümler verimli bir şekilde kapsayıcı ölçeklenebilirlik ve performans gereksinimlerini karşılamak için fiziksel bölümler (sunucu altyapısı) üzerine yerleşimini yönetir. Uygulamanın aktarım hızı ve depolama gereksinimlerini arttıkça Cosmos DB, otomatik olarak yükü sunucular arasında daha fazla sayıda yaymak için mantıksal bölümleri taşır. Cosmos DB bölüm nasıl yönettiği hakkında daha fazla bilgi için bkz. [mantıksal bölümler](partition-data.md) makalesi. Derleme veya uygulamalarınızı çalıştırmak için bu Ayrıntılar anlamak gerekli değildir.

Cosmos DB, mantıksal bölümleri fiziksel bölümler arasında yaymak için karma tabanlı bölümleme kullanır.  Bölüm anahtarı değeri öğenin Cosmos DB tarafından karılır ve karma sonucu fiziksel bölüm belirler. Cosmos DB bölüm anahtarı alanının anahtar karmaları 'N' sayıda fiziksel bölümler arasında eşit olarak ayırır.

Tek bir bölüm içindeki verilere erişen birden çok bölüm erişim sorguları daha uygun maliyetli sorgulardır. İşlemleri (saklı yordamları ve Tetikleyicileri) yalnızca tek bir mantıksal bölüm içindeki öğeleri karşı izin verilir.  

## <a id="choose-partitionkey"></a>Bir bölüm anahtarı seçmeyi

Aşağıdaki ayrıntıları bir bölüm anahtarı seçerken göz önünde bulundurun:

* Tek bir mantıksal bölüm 10 GB depolama alanı üst sınır izin verilir.  

* Bölümlenmiş kapsayıcılar ile en düşük aktarım hızı 400 RU/sn yapılandırılır. Aynı bölüm anahtarına istekleri bir bölümü için ayrılan aktarım hızı aşamaz. Bunlar ayrılan aktarım hızını aşarsa, isteklerin oranı sınırlı olur. Bu nedenle, uygulamanızda "etkin nokta" neden olmayan bir bölüm anahtarı seçmek önemlidir.

* Tüm bölümler arasında ve eşit olarak zaman içinde iş yükü eşit olarak yayılır bir bölüm anahtarı seçin.  Bölüm anahtarı tercih ettiğiniz verimli bölüm sorgular ve/veya ölçeklenebilirlik elde etmek için öğeleri birden çok bölümde dağıtma hedefe karşı işlemleri gereksinimini bakiye.

* Çok çeşitli değerleri ve mantıksal bölümler arasında eşit olarak yayılır erişim desenleri olan bir bölüm anahtarı seçin. Böylece bu kaynaklara veri depolama ve aktarım hızı için mantıksal bölümler arasında dağıtılabilir temel veriler ve etkinliğin kapsayıcınızda mantıksal bölümler kümesi arasında yaymak için olur.

* Bölüm anahtarları için aday sık sorgularınızı filtre olarak görüntülenen özelliklerini içerebilir. Sorguları içinde filtre koşulu bölüm anahtarını dahil ederek, verimli bir şekilde yeniden yönlendirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [bölümleri](partition-data.md)
* Hakkında bilgi edinin [Azure Cosmos DB'de sağlanan aktarım hızı](request-units.md)
* Hakkında bilgi edinin [Azure Cosmos DB'de küresel dağıtım](distribute-data-globally.md)
