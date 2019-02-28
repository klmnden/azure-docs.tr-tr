---
title: Bölümleme ve Azure Cosmos DB'de yatay ölçeklendirme
description: Azure Cosmos DB, bölümleme yapılandırma ve bölüm anahtarları ve uygulamanız için doğru bölüm anahtarı seçme içinde bölümleme nasıl çalıştığı hakkında bilgi edinin.
ms.author: mjbrown
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: 4c6847d8f870c02228aa14ab9e11c85b091ec48b
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56959961"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Bölümleme ve Azure Cosmos DB'de yatay ölçeklendirme

Bu makalede, Azure Cosmos DB'de fiziksel ve mantıksal bölümleri açıklanmaktadır. Bölümleme ve ölçeklendirme için en iyi uygulamalar ele alınmaktadır. 

## <a name="logical-partitions"></a>Mantıksal bölümler

Bir mantıksal bölüm aynı bölüm anahtarına sahip öğeleri kümesinden oluşur. Örneğin, burada tüm öğeleri içeren bir kapsayıcıdaki bir `City` kullanabileceğiniz özelliği `City` kapsayıcısı için bölüm anahtarı olarak. Gruplar için belirli değerlere sahip öğeleri `City`, gibi `London`, `Paris`, ve `NYC`, farklı bir mantıksal bölüm oluşturur. Temel alınan verileri silindiğinde bir bölüm silme hakkında endişelenmeniz gerekmez.

Azure Cosmos DB içinde bir kapsayıcı ölçeklenebilirlik temel birimidir. Kapsayıcı ve kapsayıcıdaki sağlama aktarım hızı eklenen veriler olduğundan otomatik olarak bir dizi mantıksal bölümler bölümlenmiş (yatay). Veri ve üretilen işi belirttiğiniz bölüm anahtarı Azure Cosmos DB kapsayıcısı için göre bölümlenir. Daha fazla bilgi için [bir Azure Cosmos DB kapsayıcısı oluşturma](how-to-create-container.md).

Bir mantıksal bölüm veritabanı işlemleri kapsamını tanımlar. Anlık görüntü yalıtımıyla bir işlemi kullanarak mantıksal bölüm içindeki öğeleri güncelleştirebilirsiniz. Kapsayıcıya yeni öğeler eklendiğinde, yeni mantıksal bölümleri şeffaf bir şekilde sistem tarafından oluşturulur.

## <a name="physical-partitions"></a>Fiziksel bölümler

Bir Azure Cosmos DB kapsayıcısı, çok sayıda mantıksal bölümler arasında veri ve üretilen işi dağıtarak ölçeklendirilir. Dahili olarak, bir veya daha fazla mantıksal bölüm çoğaltmaları olarak da adlandırılan, bir dizi içeren bir fiziksel bölüm eşlendiğine bir *çoğaltma kümesine*. Her yineleme, Azure Cosmos DB veritabanı altyapısı örneğine konakları ayarlayın. Çoğaltma kümesi, fiziksel bölüm dayanıklı, yüksek oranda kullanılabilir ve tutarlı içinde depolanan verileri sağlar. Bir fiziksel bölüm, depolama ve istek birimi (RU) en uzun süreyi destekler. Bölümün depolama kotası fiziksel bölümü yaptığı her çoğaltma devralır. Tüm fiziksel bölüm çoğaltmalarını topluca fiziksel bölüm için ayrılan aktarım hızı destekler. 

Aşağıdaki görüntüde mantıksal bölümler, küresel olarak dağıtılan fiziksel bölümlere eşlendi:

![Azure Cosmos DB bölümleme gösteren görüntü](./media/partition-data/logical-partitions.png)

Bir kapsayıcı için sağlanan aktarım hızı fiziksel bölümler arasında eşit olarak bölünür. Aktarım İsteği eşit Dağıt değil bir bölüm temel tasarım "sıcak" bölümleri oluşturabilirsiniz. Etkin bölümler, oran sınırlandırma ve sağlanan aktarım hızı verimsiz kullanılmasına neden olabilir.

Mantıksal bölümleri farklı olarak, bir iç uygulama sisteminin fiziksel bölümlerdir. Boyut, yerleştirme veya fiziksel bölüm sayısı denetleyemezsiniz ve mantıksal bölümleri ve fiziksel bölümler arasındaki eşleme denetleyemezsiniz. Ancak, mantıksal bölüm sayısı ve veri aktarım hızı ve dağıtım için doğru bölüm anahtarının seçerek denetleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [bir bölüm anahtarı seçmeyi](partitioning-overview.md#choose-partitionkey).
* Hakkında bilgi edinin [Azure Cosmos DB'de sağlanan aktarım hızı](request-units.md).
* Hakkında bilgi edinin [Azure Cosmos DB'de global dağıtım](distribute-data-globally.md).
* Bilgi edinmek için nasıl [bir Azure Cosmos DB kapsayıcısında aktarım sağlama](how-to-provision-container-throughput.md).
* Bilgi edinmek için nasıl [sağlama aktarım hızı bir Azure Cosmos DB veritabanındaki](how-to-provision-database-throughput.md).
