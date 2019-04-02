---
title: Azure Cosmos DB bölümleme
description: Azure Cosmos DB'de bölümleme genel bakış.
ms.author: rimman
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/31/2019
ms.openlocfilehash: e88be8e7b94566ff94dd94a8679f8ade9d54c0b6
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762336"
---
# <a name="partitioning-in-azure-cosmos-db"></a>Azure Cosmos DB bölümleme

Azure Cosmos DB, uygulamanızın performans gereksinimlerini karşılamak için bir veritabanında tek tek kapsayıcı ölçeklendirme işleminden bölümleme kullanır. Bölümlemede, bir kapsayıcı öğeler adlı ayrı altkümelere, bölünmüş *mantıksal bölümler*. Mantıksal bölümleri biçimlendirilmiş tabanlı değerini temel bir *bölüm anahtarı* bir kapsayıcıdaki her bir öğesi ile ilişkili. Mantıksal bir bölümdeki tüm öğeleri aynı bölüm anahtarı değerine sahip.

Örneğin, bir kapsayıcı öğeleri içerir. Her öğe için benzersiz bir değere sahip `UserID` özelliği. Varsa `UserID` bölümü olarak görev yapar anahtar kapsayıcı öğeleri için ve 1000 benzersiz `UserID` değerleri, 1.000 mantıksal bölümler için kapsayıcı oluşturulur.

Öğesinin mantıksal bölüm belirleyen bir bölüm anahtarı yanı sıra her bir kapsayıcı öğede sahip bir *kimliği öğe* (bir mantıksal bölüm içinde benzersiz). Bölüm anahtarı ve öğesi kimliği oluşturur öğenin *dizin*, benzersiz olarak tanımlayan bir öğe.

[Bir bölüm anahtarı seçmeyi](partitioning-overview.md#choose-partitionkey) uygulamanızın performansını etkileyen önemli bir karardır.

## <a name="managing-logical-partitions"></a>Mantıksal bölümleri yönetme

Azure Cosmos DB, şeffaf ve otomatik olarak mantıksal bölümler verimli bir şekilde kapsayıcı ölçeklenebilirlik ve performans gereksinimlerini karşılamak için fiziksel bölümlerde yerleşimini yönetir. Bir uygulamanın aktarım hızı ve depolama gereksinimleri arttıkça, Azure Cosmos DB, otomatik olarak yükü sunucular arasında daha fazla sayıda yaymak için mantıksal bölümleri taşır. 

Azure Cosmos DB, mantıksal bölümleri fiziksel bölümler arasında yaymak için karma tabanlı bölümleme kullanır. Azure Cosmos DB bölüm anahtarı değeri öğenin karma hale getirir. Karma sonucu fiziksel bölüm belirler. Ardından, Azure Cosmos DB bölüm anahtarı alanının anahtar karmaları eşit fiziksel bölümler arasında ayırır.

Tek bir mantıksal bölüm içindeki verilere erişen sorgular birden çok bölüm erişim sorguları daha uygun maliyetlidir. Yalnızca tek bir mantıksal bölüm öğeleri karşı işlemleri (saklı yordamları ve Tetikleyicileri) izin verilir.

Azure Cosmos DB bölüm nasıl yönettiği hakkında daha fazla bilgi için bkz. [mantıksal bölümler](partition-data.md). (Bu derleme veya uygulamalarınızı çalıştırmak için iç ayrıntılarını anlamak gerekli değildir, ancak buraya merak Okuyucu için eklenen girişlerini kolaylaştırır.)

## <a id="choose-partitionkey"></a>Bir bölüm anahtarı seçmeyi

Bir bölüm anahtarı seçmeyi için iyi bir kılavuz aşağıda verilmiştir:

* Tek bir mantıksal bölüm 10 GB depolama alanı üst sınır vardır.  

* Azure Cosmos kapsayıcılar en düşük aktarım hızı 400 istek birimi (RU/sn) saniyede içerir. Aynı bölüm anahtarına istekleri bir bölümü için ayrılan aktarım hızı aşamaz. İstekleri ayrılan aktarım hızını aşarsa, istek oranı sınırlı. Bu nedenle, uygulamanızda "etkin nokta" neden olmayan bir bölüm anahtarı seçmek önemlidir.

* Çok çeşitli değerleri ve mantıksal bölümler arasında eşit olarak yayılır erişim desenleri olan bir bölüm anahtarı seçin. Veri depolama ve aktarım hızı için kaynakları mantıksal bölümler arasında dağıtılabilir böylece bu veriler ve etkinliğin kapsayıcınızda mantıksal bölümler kümesi arasında yayılabilir yardımcı olur.

* Tüm bölümler arasında ve eşit olarak zaman içinde iş yükü eşit olarak yayılır bir bölüm anahtarı seçin. Bölüm anahtarı tercih ettiğiniz verimli bölüm sorguları ve ölçeklenebilirlik elde etmek için öğeleri birden çok bölümde dağıtma hedefe karşı işlemleri gereksinimini bakiye.

* Bölüm anahtarları için aday sık sorgularınızı filtre olarak görüntülenen özelliklerini içerebilir. Sorguları içinde filtre koşulu bölüm anahtarını dahil ederek, verimli bir şekilde yeniden yönlendirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [bölümleme ve yatay ölçeklendirme Azure Cosmos DB'de](partition-data.md).
* Hakkında bilgi edinin [Azure Cosmos DB'de sağlanan aktarım hızı](request-units.md).
* Hakkında bilgi edinin [Azure Cosmos DB'de global dağıtım](distribute-data-globally.md).
