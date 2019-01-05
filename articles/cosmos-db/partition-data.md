---
title: Bölümleme ve Azure Cosmos DB'de yatay ölçeklendirme
description: Azure Cosmos DB, bölümleme yapılandırma ve bölüm anahtarları ve uygulamanız için doğru bölüm anahtarı seçmek nasıl bölümleme nasıl çalıştığı hakkında bilgi edinin.
ms.author: mjbrown
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: 667dbab6613b83d0e72762b8bbad738d0d7697d8
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54034446"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Bölümleme ve Azure Cosmos DB'de yatay ölçeklendirme

Bu makalede, Azure Cosmos DB ve bölümleme ve ölçeklendirme için en iyi fiziksel ve mantıksal bölümleri hakkında açıklanmaktadır. 

## <a name="logical-partitions"></a>Mantıksal bölümler

Bir mantıksal bölüm aynı bölüm anahtarına bir öğe kümesi oluşur. Örneğin, burada tüm öğeleri içeren bir kapsayıcıya göz önünde bir `City` özelliği, sonra kullanabileceğiniz `City` kapsayıcısı için bölüm anahtarı olarak. Öğe için belirli değerlerle gruplarını `City` "London", "İstanbul", "NYC" vb. gibi farklı bir mantıksal bölüm oluşturacak.

Azure Cosmos DB içinde bir kapsayıcı ölçeklenebilirlik temel birimidir. Kapsayıcı ve kapsayıcıdaki sağlama aktarım hızı için eklenen veriler otomatik olarak olan mantıksal bölümler bir dizi (yatay) bölümlenmiş. Bunlar, belirttiğiniz bölüm anahtarı Cosmos kapsayıcısı için göre bölümlenir. Daha fazla bilgi için bkz. [Cosmos kapsayıcınız için bölüm anahtarını belirtebilmek nasıl](how-to-create-container.md) makalesi.

Bir mantıksal bölüm veritabanı işlemleri kapsamını tanımlar. Anlık görüntü yalıtımıyla bir işlemi kullanarak mantıksal bölüm içindeki öğeleri güncelleştirebilirsiniz.

Kapsayıcıya yeni öğeler eklendiğinde veya kapsayıcıdaki sağlanmış olan aktarım hızı artırılırsa, yeni mantıksal bölümleri şeffaf bir şekilde sistem tarafından oluşturulur.

## <a name="physical-partitions"></a>Fiziksel bölümler

Bir Cosmos kapsayıcı, çok sayıda mantıksal bölümler arasında veri ve üretilen işi dağıtarak ölçeklendirilir. Bir veya daha fazla mantıksal bölümler için dahili olarak, eşlenen bir **fiziksel bölüm** bir dizi çoğaltma kümesi olarak da adlandırılan çoğaltmaları oluşur. Her bir çoğaltma kümesi Cosmos Veritabanı Altyapısı'nın bir örneğini barındıran. Çoğaltma kümesi fiziksel bölüm dayanıklı, yüksek oranda kullanılabilir ve tutarlı içinde depolanan verileri sağlar. Bir fiziksel bölüm, depolama ve RU sabit, en yüksek miktarda destekler. Fiziksel bölüm kapsayan her çoğaltma depolama kotası devralır. Ve tüm fiziksel bölüm çoğaltmalarını topluca fiziksel bölüm için ayrılan aktarım hızı destekler. Aşağıdaki görüntüde mantıksal bölümler, küresel olarak dağıtılan fiziksel bölümlere eşlendi:

![Azure Cosmos DB bölümleme](./media/partition-data/logical-partitions.png)

Bir kapsayıcı için sağlanan aktarım hızı fiziksel bölümler arasında eşit olarak bölünür. Bu nedenle işleme istekleri eşit Dağıt değil bir bölüm temel tasarım "sıcak" Bölüm oluşturabilirler. Etkin bölümler, sağlanan aktarım hızını sınırlamayı ve verimsiz kullanımını neden olabilir.

Mantıksal bölümleri farklı olarak, bir iç uygulama sisteminin fiziksel bölümlerdir. Bunların boyutu, yerleştirme, sayı veya mantıksal bölümler ve fiziksel bölümler arasındaki eşleme denetleyemezsiniz. Ancak, mantıksal bölüm sayısı ve veri aktarım hızı ve dağıtım için doğru bölüm anahtarının seçerek denetleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Cosmos DB'de bölümleme ve ölçeklendirme için verileri bölümleme ve en iyi bir genel bakış sağlanır. 

* Hakkında bilgi edinin [Azure Cosmos DB'de sağlanan aktarım hızı](request-units.md)
* Hakkında bilgi edinin [Azure Cosmos DB'de küresel dağıtım](distribute-data-globally.md)
* Hakkında bilgi edinin [bir bölüm anahtarı seçmeyi](partitioning-overview.md#choose-partitionkey)
* Bilgi [Cosmos kapsayıcısında aktarım hızını sağlamasını yapma](how-to-provision-container-throughput.md)
* Bilgi [Cosmos veritabanı aktarım hızını sağlamasını yapma](how-to-provision-database-throughput.md)
